# System.ComponentModel.Design.Serialization.DesignerSerializationManager class

The <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager> interface is designed to be a format-independent interface to an object that controls serialization. It essentially provides context and services to serializers, which actually perform the deserialization. <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager> assists in the deserialization process by keeping track of objects. This is similar in technique to the <xref:System.ComponentModel.Design.IDesignerHost> interface: designers actually provide the user interface (UI), and <xref:System.ComponentModel.Design.IDesignerHost> provides the glue that allows different designers to work together.

The <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager> class implements <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager>. It's designed to provide a generic form of deserialization that is similar to run-time serializers like the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>.

The <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager> class achieves three goals:

- It is a simple, turnkey object that can be used to deserialize a variety of formats.

- It is generic and not tied to any particular format. It can be used equally for CodeDOM deserialization as well as markup deserialization.

- It is extensible and supports different serialization methods that are used in copy/paste and undo/redo scenarios.

Design-time serialization has the following differences from run-time object serialization:

- The object performing the serialization is generally separate from the run-time object, so that design-time logic can be removed from a component.

- The serialization scheme assumes the object will be created fully initialized, and then modified through property and method invocations during deserialization.

- Properties of an object that have values that were never set on the object (the properties contain the default values) are not serialized. Conversely, the deserialization stream may have holes.

- Emphasis is placed on the quality of the content within the serialization stream, rather than the full serialization of an object. This means that if there is no defined way to serialize an object, that object may be skipped rather than throwing an exception. The serialization engine may provide heuristics here to decide which failures can be ignored and which are unrecoverable.

- The serialization stream may have more data than is needed for deserialization. Source code serialization, for example, has user code mixed in with the code needed to deserialize an object graph. This user code must be ignored on deserialization and preserved on serialization.

Because of these differences, a different serialization model applies to design-time serialization. This model utilizes a separate serializer object for each data type being serialized. Each serializer provides its small contribution to the problem as a whole. These serializers are all coordinated through a common serialization manager. The serialization manager is responsible for maintaining state between these different serializers. As an example, consider the following class:

:::code language="csharp" source="~/snippets/csharp/System.ComponentModel.Design.Serialization/DesignerSerializationManager/Overview/Program.cs" id="Snippet2":::
:::code language="vb" source="~/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.Design.Serialization.DesignerSerializationManager.SampleObject/VB/Program.vb" id="Snippet2":::

An instance of this class would utilize three different serializers: one for `SampleObject`, one for strings, and another for integers. The serializer for `SampleObject` is called the root serializer because `SampleObject` is the root of the serialization graph. More complex object graphs can be created as well. For example, consider what would happen if `SampleObject` were changed as follows:

:::code language="csharp" source="~/snippets/csharp/System.ComponentModel.Design.Serialization/DesignerSerializationManager/Overview/SampleObject.cs" id="Snippet11":::
:::code language="vb" source="~/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.Design.Serialization.DesignerSerializationManager.SampleObject/VB/SampleObject.vb" id="Snippet11":::

This allows `SampleObject` to have a child that is another instance of itself. The following code fills in the object graph:

:::code language="csharp" source="~/snippets/csharp/System.ComponentModel.Design.Serialization/DesignerSerializationManager/Overview/SampleObject.cs" id="Snippet12":::
:::code language="vb" source="~/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.Design.Serialization.DesignerSerializationManager.SampleObject/VB/SampleObject.vb" id="Snippet12":::

When `root` is serialized, there will be four serializers used: one root serializer, one serializer for the child `SampleObject`, one serializer for `int`, and one serializer for `string`. Serializers are cached based on type, so there is no need to create a serializer for each instance of `SampleObject`.

The <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager> class is based on the idea of a serialization session. A session maintains state that can be accessed by the various serializers. When a session is disposed, this state is destroyed. This helps to ensure that serializers remain largely stateless, and helps to clean up serializers that are have been corrupted. The following tables describe how state is managed in and among sessions.

## Global state

This state is owned by the serialization manager object, but is independent of the current serialization session.

|Object|Usage|
|------------|-----------|
|Serialization providers|Objects can add themselves as custom serialization providers. Because these providers are used to locate serializers, they outlive a serialization session.|

## Session-owned state

This state is owned by a session and is destroyed when a session is destroyed. Consequently, accessing any properties or methods that would manipulate this state will throw an exception if the serialization manager is not in an active session.

|Object|Usage|
|------------|-----------|
|<xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager.ResolveName> event|The <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager.ResolveName> event is attached by a serializer to provide additional resolution of names. All handlers are detached from this event when a session terminates.|
|<xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager.SerializationComplete> event|The <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager.SerializationComplete> event is raised just before a session is disposed. Then, all handlers are detached from this event.|
|Name table|The serialization manager maintains a table that maps between objects and their names. Serializers may give objects names for easy identification. This name table is cleared when the session terminates.|
|Serializer cache|The serialization manager maintains a cache of serializers it has been asked to supply. This cache is cleared when the session terminates. The public <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager.GetSerializer%2A> method can safely be called at any time, but its value is cached only if it is called from within a session.|
|Context stack|The serialization manager maintains an object called the context stack, which you can access with the <xref:System.ComponentModel.Design.Serialization.IDesignerSerializationManager.Context%2A> property. Serializers can use this stack to store additional information that is available to other serializers. For example, a serializer that is serializing a property value can push the property name on the serialization stack before asking the value to serialize. This stack is cleared when the session is terminated.|
|Error list|The serialization manager maintains a list of errors that occurred during serialization. This list, which is accessed through the <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager.Errors%2A> property, is cleared when the session is terminated. Accessing the <xref:System.ComponentModel.Design.Serialization.DesignerSerializationManager.Errors%2A> property between sessions will result in an exception.|
