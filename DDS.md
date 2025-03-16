- DDS, or Data Distribution Service, is a middleware protocol and API standard for data-centric connectivity. It enables scalable, real-time, dependable, high-performance, and interoperable data exchanges using a publish-subscribe pattern. DDS employs the Interface Description Language (IDL) defined by the Object Management Group (OMG) for message definition and serialization.
- The consideration to integrate DDS into ROS stems from the need for an end-to-end middleware solution. Such a solution reduces the amount of custom code ROS needs to maintain and leverages existing, well-documented standards. DDS's design aligns closely with ROS's requirements, offering features like distributed discovery and a publish-subscribe transport mechanism.

 **Core Features of DDS**

1. **Publish-Subscribe Transport:** Similar to ROS's mechanism, facilitating decoupled communication between components.
2. **Interface Description Language (IDL):** Used for defining messages and ensuring consistent serialization across platforms.
3. **Distributed Discovery:** Allows DDS applications to locate each other without a centralized system like the ROS master, enhancing fault tolerance.
4. **DDS-RPC:** A request-response transport mechanism, akin to ROS's service system, introduced in beta 2 as of June 2016.
- Since DDS is implemented, by default, on UDP, it does not depend on a reliable transport or hardware for communication. This means that DDS has to reinvent the reliability wheel (basically TCP plus or minus some features), but in exchange DDS gains portability and control over the behavior. Control over several parameters of reliability, what DDS calls Quality of Service (QoS), gives maximum flexibility in controlling the behavior of communication. For example, if you are concerned about latency, like for soft real-time, you can basically tune DDS to be just a UDP blaster. In another scenario you might need something that behaves like TCP, but needs to be more tolerant to long dropouts, and with DDS all of these things can be controlled by changing the QoS parameters.
- DDS provides discovery, message definition, message serialization, and publish-subscribe transport.

**Publish/Subscribe System (Nodes & Topics Work Differently)**

- In ROS 1, a node directly publishes or subscribes to a topic.
- In ROS 2 (with DDS), there are extra layers:
    - **Graph Participant** → Like a ROS Node, but it just exists in the system.
    - **Topic** → Just like ROS topics, but they exist as independent objects.
    - **Publisher & Subscriber** → These connect to topics, just like in ROS 1.
    - **DataWriter & DataReader** → Extra DDS layers that actually send/receive the data.
