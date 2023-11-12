---
layout: '../../layouts/Layout.astro'
pattern: 'iterator'
---

## Iterator Pattern Example
    

An iterator is an object that provides a common interface for traversing elements of a collection. The iterator has methods such as `next()` to retrieve the next element from the collection and `hasNext()` to check if there are still elements to iterate through. The iterator is responsible for keeping track of the current state of the iteration.

The central idea of the Iterator pattern is to extract the traversal behavior of a collection and place it in a separate object called an iterator.

In addition to implementing the traversal algorithm itself, an iterator object encapsulates all the details of the traversal, such as the current position and how many elements are left until the end. Because of this, multiple iterators can traverse the same collection at the same time, independently of each other. Typically, iterators provide a primary method for extracting elements from the collection. The client can continue calling this method until it returns nothing, which means the iterator has traversed all the elements.

**STRUCTURE**

<div align="center">
<img src="https://refactoring.guru/images/patterns/diagrams/iterator/structure.png?id=35ea851f8f6bbe51d79eb91e6e6519d0" alt="STRUCTURE">
</div>


---


The following example example was extracted from **Deeplearning4j (DL4J)** a deep learning and machine learning library developed in Java. It is designed to enable the creation, training, and deployment of artificial neural networks and machine learning models in Java applications. DL4J stands out as one of the few deep learning libraries that integrates well into the Java ecosystem and is suitable for enterprise and production applications.

The design pattern is found in the class: `WeightedRandomWalkIterator`.

This class serves as the iterator and adheres to the `GraphWalkIterator` interface. It represents the iterator used for traversing the graph using weighted random walks.

```java
```java
public class WeightedRandomWalkIterator<V> implements GraphWalkIterator<V> {
    private final IGraph<V,  extends Number> graph;
    private final int walkLength;
    private final NoEdgeHandling mode;
    private final int firstVertex;
    private final int lastVertex;

    private int position;
    private Random rng;
    private int[] order;

    public WeightedRandomWalkIterator(IGraph<V, ? extends Number> graph, int walkLength) {
        this(graph, walkLength, System.currentTimeMillis(), NoEdgeHandling.EXCEPTION_ON_DISCONNECTED);
    }
```
Subsequently, in the `IVertexSequence` class, methods such as `next()` for obtaining the next element and `hasNext()` for verifying if there are more elements to iterate over are included, as specified by the design pattern.
 
```java
```java
 public IVertexSequence<V> next() {
        if (!hasNext())
            throw new NoSuchElementException();
        //Generate a weighted random walk starting at vertex order[current]
        int currVertexIdx = order[position++];
        int[] indices = new int[walkLength + 1];
        indices[0] = currVertexIdx;
        if (walkLength == 0)
            return new VertexSequence<>(graph, indices);

        for (int i = 1; i <= walkLength; i++) {
            List<? extends Edge<? extends Number>> edgeList = graph.getEdgesOut(currVertexIdx);

            //First: check if there are any outgoing edges from this vertex. If not: handle the situation
            if (edgeList == null || edgeList.isEmpty()) {
                switch (mode) {
                    case SELF_LOOP_ON_DISCONNECTED:
                        for (int j = i; j < walkLength; j++)
                            indices[j] = currVertexIdx;
                        return new VertexSequence<>(graph, indices);
                    case EXCEPTION_ON_DISCONNECTED:
                        throw new NoEdgesException("Cannot conduct random walk: vertex " + currVertexIdx
                                        + " has no outgoing edges. "
                                        + " Set NoEdgeHandling mode to NoEdgeHandlingMode.SELF_LOOP_ON_DISCONNECTED to self loop instead of "
                                        + "throwing an exception in this situation.");
                    default:
                        throw new RuntimeException("Unknown/not implemented NoEdgeHandling mode: " + mode);
                }
            }

            //To do a weighted random walk: we need to know total weight of all outgoing edges
            double totalWeight = 0.0;
            for (Edge<? extends Number> edge : edgeList) {
                totalWeight += edge.getValue().doubleValue();
            }

            double d = rng.nextDouble();
            double threshold = d * totalWeight;
            double sumWeight = 0.0;
            for (Edge<? extends Number> edge : edgeList) {
                sumWeight += edge.getValue().doubleValue();
                if (sumWeight >= threshold) {
                    if (edge.isDirected()) {
                        currVertexIdx = edge.getTo();
                    } else {
                        if (edge.getFrom() == currVertexIdx) {
                            currVertexIdx = edge.getTo();
                        } else {
                            currVertexIdx = edge.getFrom(); //Undirected edge: might be next--currVertexIdx instead of currVertexIdx--next
                        }
                    }
                    indices[i] = currVertexIdx;
                    break;
                }
            }
        }
        return new VertexSequence<>(graph, indices);
    }
```

The code also includes other classes that correctly complete the functionality of the iterator.

To access the complete source code for both the iterator and the **Deeplearning4j (DL4J)** library, please follow the links:

Link to the `WeightedRandomWalkIterator` class: [WeightedRandomWalkIterator Source](https://github.com/deeplearning4j/deeplearning4j/blob/master/deeplearning4j/deeplearning4j-graph/src/main/java/org/deeplearning4j/graph/iterator/WeightedRandomWalkIterator.java)

Link to the **Deeplearning4j (DL4J)** repository: [DL4J Repository](https://github.com/deeplearning4j/deeplearning4j/tree/master)
