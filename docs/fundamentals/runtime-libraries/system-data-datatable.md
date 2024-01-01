# System.Data.DataTable class

The <xref:System.Data.DataTable> is a central object in the ADO.NET library. Other objects that use the <xref:System.Data.DataTable> include the <xref:System.Data.DataSet> and the <xref:System.Data.DataView>.

When accessing <xref:System.Data.DataTable> objects, note that they are conditionally case sensitive. For example, if one <xref:System.Data.DataTable> is named "mydatatable" and another is named "Mydatatable", a string used to search for one of the tables is regarded as case sensitive. However, if "mydatatable" exists and "Mydatatable" does not, the search string is regarded as case insensitive. A <xref:System.Data.DataSet> can contain two <xref:System.Data.DataTable> objects that have the same <xref:System.Data.DataTable.TableName%2A> property value but different <xref:System.Data.DataTable.Namespace%2A> property values. For more information about working with <xref:System.Data.DataTable> objects, see [Creating a DataTable](/dotnet/framework/data/adonet/dataset-datatable-dataview/creating-a-datatable).

If you're creating a <xref:System.Data.DataTable> programmatically, you must first define its schema by adding <xref:System.Data.DataColumn> objects to the <xref:System.Data.DataColumnCollection> (accessed through the <xref:System.Data.DataTable.Columns%2A> property). For more information about adding <xref:System.Data.DataColumn> objects, see [Adding Columns to a DataTable](/dotnet/framework/data/adonet/dataset-datatable-dataview/adding-columns-to-a-datatable).

To add rows to a <xref:System.Data.DataTable>, you must first use the <xref:System.Data.DataTable.NewRow%2A> method to return a new <xref:System.Data.DataRow> object. The <xref:System.Data.DataTable.NewRow%2A> method returns a row with the schema of the <xref:System.Data.DataTable>, as it is defined by the table's <xref:System.Data.DataColumnCollection>. The maximum number of rows that a <xref:System.Data.DataTable> can store is 16,777,216. For more information, see [Adding Data to a DataTable](/dotnet/framework/data/adonet/dataset-datatable-dataview/adding-data-to-a-datatable).

The <xref:System.Data.DataTable> also contains a collection of <xref:System.Data.Constraint> objects that can be used to ensure the integrity of the data. For more information, see [DataTable Constraints](/dotnet/framework/data/adonet/dataset-datatable-dataview/datatable-constraints).

There are many <xref:System.Data.DataTable> events that can be used to determine when changes are made to a table. These include <xref:System.Data.DataTable.RowChanged>, <xref:System.Data.DataTable.RowChanging>, <xref:System.Data.DataTable.RowDeleting>, and <xref:System.Data.DataTable.RowDeleted>. For more information about the events that can be used with a <xref:System.Data.DataTable>, see [Handling DataTable Events](/dotnet/framework/data/adonet/dataset-datatable-dataview/handling-datatable-events).

When an instance of <xref:System.Data.DataTable> is created, some of the read/write properties are set to initial values. For a list of these values, see the <xref:System.Data.DataTable.%23ctor%2A> constructor.

> [!NOTE]
> The <xref:System.Data.DataSet> and <xref:System.Data.DataTable> objects inherit from <xref:System.ComponentModel.MarshalByValueComponent> and support the <xref:System.Runtime.Serialization.ISerializable> interface for .NET Framework remoting. These are the only ADO.NET objects that you can use for .NET Framework remoting.

## Security considerations

For information about DataSet and DataTable security, see [Security guidance](/dotnet/framework/data/adonet/dataset-datatable-dataview/security-guidance).

## Examples

The following example creates two <xref:System.Data.DataTable> objects and one <xref:System.Data.DataRelation> object, and adds the new objects to a <xref:System.Data.DataSet>. The tables are then displayed in a <xref:System.Windows.Forms.DataGridView> control.

:::code language="csharp" source="~/snippets/csharp/VS_Snippets_ADO.NET/Classic WebData DataTable Example/CS/source.cs" id="Snippet1":::
:::code language="vb" source="~/snippets/visualbasic/VS_Snippets_ADO.NET/Classic WebData DataTable Example/VB/source.vb" id="Snippet1":::

This sample demonstrates how to create a DataTable manually with specific schema definitions:

- Create multiple DataTables and define the initial columns.
- Create the table constraints.
- Insert the values and display the tables.
- Create the expression columns and display the tables.

:::code language="csharp" source="~/snippets/csharp/VS_Snippets_ADO.NET/classic webdata datatable example2/cs/source.cs" id="Snippet1":::
:::code language="vb" source="~/snippets/visualbasic/VS_Snippets_ADO.NET/classic webdata datatable example2/vb/source.vb" id="Snippet1":::
