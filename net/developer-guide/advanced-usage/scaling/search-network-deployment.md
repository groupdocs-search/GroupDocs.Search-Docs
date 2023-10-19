---
id: search-network-deployment
url: search/net/search-network-deployment
title: Search network deployment
weight: 20
description: "This page contains information about deployment of the search network."
keywords: search network deployment, deployment of search network, distributed index deployment, deployment of distributed index
productName: GroupDocs.Search for .NET
hideChildren: False
---
The GroupDocs.Search search network consists of one master node and several slave nodes. Each node is a hardware server with the .NET platform deployed on it. Inside the .NET platform, an application is deployed that creates an instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class.

An instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class provides the services described in the configuration:
 - Indexing service;
 - Search services;
 - Extraction services;
 - Shard services.
Indexing and search services also provide an interface to access all search network features.

To deploy the GroupDocs.Search search network, you must perform the following steps:
 - On each slave server, create an instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class, call the Start method.
 - On the master server, create an instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class, passing the configuration object to the constructor.
 - Call the [ConfigureAllNodes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configureallnodes/) method of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) object on the master server.
 - Call the [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/) method of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) object on the master server.

The following code example demonstrates starting a slave node on the server.

**C#**

```csharp
SearchNetworkNode node1 = new SearchNetworkNode(
    1,
    basePath + "Node1",
    new TcpSettings(basePort + 1, sendTimeout, receiveTimeout));
node1.Start();
```

The following code example demonstrates starting a master node on the server.

**C#**

```csharp
SearchNetworkNode node0 = new SearchNetworkNode(
    0,
    basePath + "Node0",
    new TcpSettings(basePort, sendTimeout, receiveTimeout),
    new ConsoleLogger(),
    configuration);

node0.Events.ConfigurationCompleted += (s, e) =>
{
    Console.WriteLine("Configuration complete");
};

Console.WriteLine();
Console.WriteLine("Configuring the search network");
node0.ConfigureAllNodes();

Console.WriteLine("Launching the search network");
node0.Start();
```

The following code example demonstrates the implementation of a simple console logger.

**C#**

```csharp
class ConsoleLogger : ILogger
{
    public void Error(string message)
    {
        Console.WriteLine("Error: " + message);
    }

    public void Trace(string message)
    {
        Console.WriteLine("Trace: " + message);
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
