# Versioning

The Eight Fallacies of Distributed Computing

Essentially everyone, when they first build a distributed application, makes the following eight assumptions. All prove to be false in the long run and all cause big trouble and painful learning experiences.

* The network is reliable
* Latency is zero
* Bandwidth is infinite
* The network is secure
* Topology doesn’t change
* There is one administrator
* Transport cost is zero
* The network is homogeneous

– Peter Deutsch

---

Mutability and immutability are both important concepts in software development.

For example:
* we need to be able to respond quickly to customer feedback (mutability)
* we can reason about something much easier if it does not change (immutability)

Versioning is the act of creating a unique identifier for a given state. We can then create a new name when making a change, while preserving the functionality of other versions.
