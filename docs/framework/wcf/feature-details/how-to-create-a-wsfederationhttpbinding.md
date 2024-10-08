---
description: "Learn more about: How to: Create a WSFederationHttpBinding"
title: "How to: Create a WSFederationHttpBinding"
ms.date: "03/30/2017"
dev_langs:
  - "csharp"
  - "vb"
helpviewer_keywords:
  - "WCF, federation"
  - "federation"
ms.assetid: e54897d7-aa6c-46ec-a278-b2430c8c2e10
---
# How to: Create a WSFederationHttpBinding

In Windows Communication Foundation (WCF), the <xref:System.ServiceModel.WSFederationHttpBinding> class ([\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) in configuration) provides a mechanism for exposing a federated service. That is, a service that requires clients to authenticate using a security token issued by a security token service. This topic shows how to set up a <xref:System.ServiceModel.WSFederationHttpBinding> in both code and configuration. Once the binding is created, you can set up an endpoint to use that binding.

 The basic steps are outlined as follows:

1. Select a security mode. The <xref:System.ServiceModel.WSFederationHttpBinding> supports `Message`, which provides end-to-end security at the message level, even across multiple hops, and `TransportWithMessageCredential`, which provides better performance in cases where the client and the service can make a direct connection over HTTPS.

    > [!NOTE]
    > The <xref:System.ServiceModel.WSFederationHttpBinding> also supports `None` as a security mode. This mode is not secure and is provided for debugging purposes only. If a service endpoint is deployed with a <xref:System.ServiceModel.WSFederationHttpBinding> with its security mode set to `None`, the resulting client binding (generated by the [ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)) is a <xref:System.ServiceModel.WSHttpBinding> with a security mode of `None`.

     Unlike other system-provided bindings, it is not necessary to select a client credential type when using the `WSFederationHttpBinding`. This is because the client credential type is always an issued token. WCF acquires a token from a specified issuer and presents that token to the service to authenticate the client.

2. On federated clients, set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerAddress%2A> property to the URL of the security token service. Set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerBinding%2A> to the binding to use to communicate with the security token service.

3. Optional. Set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedTokenType%2A> property to the Uniform Resource Identifier (URI) of a token type. On federated services, specify the token type that the service expects. On federated clients, specify the token type the client requests from the security token service.

     If no token type is specified, clients generate WS-Trust Request Security Tokens (RSTs) without a token type URI, and services expect client authentication using a Security Assertions Markup Language (SAML) 1.1 token by default.

     The URI for a SAML 1.1 token is `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1`.

4. Optional. On federated services, set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerMetadataAddress%2A> property to the metadata URL of a security token service. The metadata endpoint enables clients of the service to select an appropriate binding/endpoint pair, if the service is configured to publish metadata. For more information about publishing metadata, see [Publishing Metadata](publishing-metadata.md).

 You can also set other properties, including the type of key used as a proof key in the issued token, the algorithm suite to use between the client and the service, whether to negotiate or explicitly specify the service credential, any specific claims the service expects the issued token to contain, and any additional XML elements that must be added to the request the client sends to the security token service.

> [!NOTE]
> The `NegotiateServiceCredential` property is only relevant when the `SecurityMode` is set to `Message`. If `SecurityMode` is set to `TransportWithMessageCredential`, then the `NegotiateServiceCredential` property is ignored.

## To configure a WSFederationHttpBinding in code

1. Create an instance of the <xref:System.ServiceModel.WSFederationHttpBinding>.

2. Set the <xref:System.ServiceModel.WSFederationHttpSecurity.Mode%2A> property to <xref:System.ServiceModel.WSFederationHttpSecurityMode> or <xref:System.ServiceModel.WSFederationHttpSecurityMode.Message> as required. If an algorithm suite other than <xref:System.ServiceModel.Security.SecurityAlgorithmSuite.Basic256%2A> is required, set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.AlgorithmSuite%2A> property to a value taken from <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>.

3. Set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.NegotiateServiceCredential%2A> property as appropriate.

4. Set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedKeyType%2A> property to <xref:System.IdentityModel.Tokens.SecurityKeyType>`SymmetricKey` or .`AsymmetricKey` as required.

5. Set the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedTokenType%2A> property to the appropriate value. If no value is set, WCF defaults to `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1`, which indicates SAML 1.1 tokens.

6. Required on the client if no local issuer is specified; optional on the service. Create an <xref:System.ServiceModel.EndpointAddress> that contains the address and identity information of the security token service and assign the <xref:System.ServiceModel.EndpointAddress> instance to the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerAddress%2A> property.

7. Required on the client if no local issuer is specified; not used on the service. Create a <xref:System.ServiceModel.Channels.Binding> for the `SecurityTokenService` and assign the <xref:System.ServiceModel.Channels.Binding> instance to the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuerBinding%2A> property.

8. Not used on the client; optional on the service. Create an <xref:System.ServiceModel.EndpointAddress> instance for the metadata of the security token service and assign it to the `IssuerMetadataAddress` property.

9. Optional on both the client and the service. Create and add one or more <xref:System.ServiceModel.Security.Tokens.ClaimTypeRequirement> instances to the collection returned by the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.ClaimTypeRequirements%2A> property.

10. Optional on both the client and the service. Create and add one or more <xref:System.Xml.XmlElement> instances to the collection returned by the <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.TokenRequestParameters%2A> property.

## To create a federated endpoint in configuration

1. Create a [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) as a child of the [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) element in the application configuration file.

2. Create a [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) element as a child of [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) and set the `name` attribute to an appropriate value.

3. Create a `<security>` element as a child of the [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) element.

4. Set the `mode` attribute on the `<security>` element to a value of `Message` or `TransportWithMessageCredential`, as required.

5. Create a `<message>` element as a child of the `<security>` element.

6. Optional. Set the `algorithmSuite` attribute on the `<message>` element with an appropriate value. The default is `Basic256`.

7. Optional. If an asymmetric proof key is required, set the `issuedKeyType` attribute of the `<message>` element to `AsymmetricKey`. The default is `SymmetricKey`.

8. Optional. Set the `issuedTokenType` attribute on the `<message>` element.

9. Required on the client if no local issuer is specified; optional on the service. Create an `<issuer>` element as a child of the `<message>` element.

10. Set the `address` attribute to the `<issuer>` element and specify the address at which the security token service accepts token requests.

11. Optional. Add an `<identity>` child element and specify the identity of the security token service

12. For more information, see [Service Identity and Authentication](service-identity-and-authentication.md).

13. Required on the client if no local issuer is specified; not used on the service. Create a [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) element in the bindings section that can be used to communicate with the security token service. For more information about creating a binding, see [How to: Specify a Service Binding in Configuration](../how-to-specify-a-service-binding-in-configuration.md).

14. Specify the binding created in the previous step by setting the `binding` and `bindingConfiguration` attributes of the `<issuer>` element.

15. Not used on the client; optional on the service. Create an `<issuerMetadata>` element as a child of the `<message>` element. Then, in an `address` attribute on the `<issuerMetadata>` element, specify the address at which the security token service is to publish its metadata. Optionally, add an `<identity>` child element and specify the identity of the security token service.

16. Optional on both the client and the service. Add a `<claimTypeRequirements>` element as a child of the `<message>` element. Specify required and optional claims that the service relies on by adding [\<add>](../../configure-apps/file-schema/wcf/add-of-claimtyperequirements.md) elements to the `<claimTypeRequirements>` element and specifying the claim type with the `claimType` attribute. Specify whether a given claim is required or optional by setting the `isOptional` attribute.

## Example

The following code sample shows code for setting up a `WSFederationHttpBinding` imperatively.

[!code-csharp[c_FederationBinding#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_federationbinding/cs/source.cs#2)]
[!code-vb[c_FederationBinding#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_federationbinding/vb/source.vb#2)]

## See also

- [Federation](federation.md)
- [Federation Sample](../samples/federation-sample.md)
- [How to: Disable Secure Sessions on a WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
