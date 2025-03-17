ROS 2 offers a rich variety of Quality of Service (QoS) policies that allow you to tune communication between nodes. With the right set of Quality of Service policies, ROS 2 can be as reliable as TCP or as best-effort as UDP, with many, many possible states in between.

A set of QoS “policies” combine to form a QoS “profile”. Given the complexity of choosing the correct QoS policies for a given scenario, ROS 2 provides a set of predefined QoS profiles for common use cases (e.g. sensor data).

## - ROS 2 QoS Policies 
1. History

   - [ ] Keep Last (N) → Stores only the last N messages.
   - [ ] Keep All → Stores all messages (limited by system resources).
2. Depth

   - [ ] Queue Size (N) → Works only if "Keep Last" is used.
3. Reliability

   - [ ] Best Effort → May lose messages if the network is unstable.
   - [ ] Reliable → Ensures messages are delivered (retries if needed).
4. Durability

   - [ ] Transient Local → Messages persist for late subscribers.
   - [ ] Volatile → Messages are not saved for late subscribers.
5. Deadline

   - [ ] Max Duration → The longest allowed time between messages.
6. Lifespan

   - [ ] Max Duration → How long a message is valid before being dropped.
7. Liveliness

   - [ ] Automatic → If any publisher in a node is active, all are considered alive.
   - [ ] Manual by Topic → Publisher must manually signal that it is still alive.
8. Lease Duration

   - [ ] Max Period → Time before a lost publisher is considered inactive.

##   Comparison to ROS 1
   The “history” and “depth” policies in ROS 2 combine to provide functionality akin to the queue size in ROS 1.

The “reliability” policy in ROS 2 is akin to the use of either UDPROS (only in roscpp) for “best effort”, or TCPROS (ROS 1 default) for “reliable”. Note however that even the reliable policy in ROS 2 is implemented using UDP, which allows for multicasting if appropriate.

The “durability” policy “transient local”, combined with any depth, provides functionality similar to that of “latching” publishers. 

## Current ROS 2 QoS Profiles Default
1. Default (Pub/Sub) → ROS 1-like behavior

   - [ ] History: Keep last (queue = 10)
   - [ ] Reliability: Reliable
   - [ ] Durability: Volatile
   - [ ] Liveliness: System default
   - [ ] Deadline, lifespan, lease duration: Default
2. Services → Reliable but avoids outdated requests

   - [ ] Reliability: Reliable
   - [ ] Durability: Volatile (prevents old requests on restart)
3. Sensor Data → Prioritizes fresh data over reliability

   - [ ] Reliability: Best effort
   - [ ] Queue: Small (low latency)
4. Parameters → Similar to services but higher queue depth

5. System Default → Uses RMW’s default QoS settings.

## QoS Compatibility
QoS profiles may be configured for publishers and subscriptions independently. A connection between a publisher and a subscription is only made if the pair has compatible QoS profiles.

QoS profile compatibility is determined based on a “Request vs Offered” model. Subscriptions request a QoS profile that is the “minimum quality” that it is willing to accept, and publishers offer a QoS profile that is the “maximum quality” that it is able to provide. Connections are only made if every policy of the requested QoS profile is not more stringent than that of the offered QoS profile.

## QoS Events (Triggers when something goes wrong)
### 1. For Publishers:

- [ ] Missed Deadline → Didn't publish a message on time.
- [ ] Liveliness Lost → Stopped signaling that it's active.
- [ ] Incompatible QoS → Found a subscriber but can’t meet its QoS requirements → No connection.
### 2. For Subscriptions:

- [ ] Missed Deadline → Didn't receive a message on time.
- [ ] Liveliness Changed → A publisher stopped signaling that it’s active.
- [ ] Incompatible QoS → Found a publisher but its QoS settings don’t match → No connection.
## Matched Events (Triggers when connections are made or lost)
- [ ] For Publishers: Detects when a subscriber connects or disconnects.
- [ ] For Subscriptions: Detects when a publisher connects or disconnects.
