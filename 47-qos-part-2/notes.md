# CCNA QoS – Revision Notes

## 1. DSCP markings

DSCP uses 6 bits in the IP header to identify traffic and indicate how it should be treated.

| Marking  | Purpose                                                      | DSCP value |
| -------- | ------------------------------------------------------------ | ---------- |
| DF (CS0) | Best-effort traffic                                          | 0          |
| EF       | Low loss, delay, and jitter; typically voice                 | 46         |
| AF       | Four traffic classes, each with three drop-precedence levels | Varies     |
| CS       | Class selector; backward compatibility with IP Precedence    | Varies     |

### Assured Forwarding (AF)

- Four classes: AF1, AF2, AF3, AF4.
- Each class has three drop-precedence levels: 1, 2, 3.
- Higher class number = higher priority.
- Higher drop precedence = more likely to be dropped during congestion.
- Format: `AFXY` — X = class, Y = drop precedence.

Calculate the decimal DSCP value:

\\[ DSCP = 8X + 2Y \\]

Examples:

| AF marking | DSCP value |
| ---------- | ---------- |
| AF11       | 10         |
| AF12       | 12         |
| AF23       | 22         |
| AF32       | 28         |
| AF43       | 38         |

- Highest AF value: AF43.
- Best treatment: AF41.
- Worst treatment: AF13.

### Class Selector (CS)

- CS provides eight values: CS0–CS7.
- Maintains backward compatibility with IP Precedence.
- Calculate decimal DSCP: `8 × CS number`.

| Marking | DSCP value |
| ------- | ---------- |
| CS0     | 0          |
| CS1     | 8          |
| CS2     | 16         |
| CS3     | 24         |
| CS4     | 32         |
| CS5     | 40         |
| CS6     | 48         |
| CS7     | 56         |

## 2. RFC 4594 – Recommended markings

| Traffic type       | Recommended marking |
| ------------------ | ------------------- |
| Voice              | EF                  |
| Interactive video  | AF4x                |
| Streaming video    | AF3x                |
| High-priority data | AF2x                |
| Best-effort data   | DF (DSCP 0)         |

These are recommendations; network engineers define the QoS policy.

## 3. Trust boundaries

A trust boundary determines whether a device trusts the QoS markings received from another device.

- Trusted markings: forwarded without changing them.
- Untrusted markings: changed according to the configured policy.
- It is generally recommended to trust markings from an IP phone.
- Configure the trust boundary on the switchport connected to the phone.
- CoS and PCP refer to the priority field in the 802.1Q VLAN tag.

CCNA focus: Understand what a trust boundary does; the transcript says configuration is not required for the exam.

## 4. Queuing and congestion management

When packets arrive faster than an interface can forward them, they wait in a queue.

- FIFO: First In, First Out.
- Tail drop: New packets are dropped when the queue is full.
- RED: Randomly drops packets early to help prevent tail drop.
- WRED: Uses packet priority to determine which traffic is more likely to be dropped.

### Multiple queues and scheduling

1. Traffic enters the device (ingress).
2. The device routes and classifies traffic.
3. Traffic is placed into the appropriate queue.
4. A scheduler selects which queue sends next.
5. Packets are transmitted.

- Weighted Round Robin: Cycles through queues, giving higher-weight queues more bandwidth.
- CBWFQ (Class-Based Weighted Fair Queuing): Uses weighted scheduling and guarantees each queue a percentage of bandwidth during congestion.
- LLQ (Low Latency Queuing): Adds strict-priority queues for delay-sensitive traffic, such as voice and video.

LLQ risk: A continuously busy priority queue can starve other queues. Policing can limit traffic in the priority queue.

## 5. Traffic shaping vs. policing

Both control the rate of traffic.

| Shaping                           | Policing                                    |
| --------------------------------- | ------------------------------------------- |
| Buffers excess traffic in a queue | Drops traffic exceeding the configured rate |
| Sends traffic later               | Drops excess traffic                        |
| Smooths traffic rate              | Enforces a traffic-rate limit               |

- Both can allow configurable bursts.
- Classification can apply different rates to different traffic types.
- Example: A customer shapes outbound traffic to 300 Mbps to match the ISP's 300 Mbps policing limit.

## Key CCNA reminders

- EF = DSCP 46; typically used for voice.
- DF = DSCP 0; best-effort traffic.
- Higher AF class = higher priority.
- Higher AF drop precedence = more likely to be dropped.
- AF DSCP calculation: `8X + 2Y`.
- CS DSCP calculation: `8 × CS number`.
- Tail drop: Queue full, new packets dropped.
- CBWFQ: Weighted scheduling with bandwidth guarantees.
- LLQ: Strict priority for delay-sensitive traffic.
- Shaping buffers; policing drops.
- Marking a packet EF does not automatically prioritize it. QoS scheduling policies such as CBWFQ and LLQ must be configured to act on the markings.