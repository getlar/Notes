
Reviewer 1:

We thank the reviewer for their detailed and supportive feedback and valuable insights. Below, we address each point raised.

The reviewer notes that the solution appears specific to grid-based games, but we argue this is not necessarily the case, as a shared feature extractor and trajectory separation can generalize beyond 2D features. 

To address the broader applicability, we clarify our methods: trajectory separation treats agents not as standalone learners with separate policies (as in independent PPO) but as implicitly connected through a shared sparse global reward structure and a shared policy and value loss. These losses are computed from multiple sub-losses based on separate batched observation-action pairs from individual trajectories. This approach enables a fine-grained gradient flow, allowing more controlled loss calculations that consider events like agent deaths and new agent spawns.

Our method can also improve training efficiency by prioritizing high-advantage or high-return data (quality data), enabling collective behavior learning not achievable with Independent PPO. We thank the reviewer for the valuable comment, and will include Independent PPO as a baseline.

The reviewer asks about the "local" field-of-view in the "centralized" model. It refers to the convolutional kernel in the shared feature extractor, which processes specific segments of the grid-based environment.

The reviewer notes that IPPO allows batching all agents in one pass. Our tests, however, showed it performed worse than separate trajectory passes (Figure 6).

The reviewer highlights the specificity of the pixel-to-pixel feature extractor to 2D grid environments. We acknowledge this limitation and agree that generalizing this aspect of the implementation is an important direction for future work.

We appreciate the suggestion to include the PPO clipping term for clarity and will revise Figure 1 accordingly.

The reviewer points out potential confusion in interpreting the 'done' signal. The initial 'True' flags indicate agents not yet spawned, while subsequent 'True' flags signify agents that have been removed. We will clarify this in the text to address the misunderstanding.

Regarding Figure 5, "1 unit groups" refers to global trajectories treating all units as one group. We acknowledge the ambiguity and will clarify the figure caption.

In summary, we thank the reviewer for their constructive feedback, which we believe will significantly improve the paper.

--------------------------------------

We thank the reviewer for their detailed feedback and valuable insights. We appreciate their patience in reviewing our work, which they found weaker in some areas. Below, we address each point raised in the review.

The solution may appear specific to grid-based top-down multi-agent strategy games like Lux, but we argue that our shared feature extractors and trajectory separation approach can generalize beyond image-like features.

To address the broader applicability, we clarify our methods: trajectory separation treats agents not as standalone learners with separate policies (as in independent PPO) but as implicitly connected through a shared sparse global reward structure and a shared policy and value loss. Factories and units, however, have their own collective losses. These losses are computed from multiple sub-losses based on separate batched observation-action pairs from individual trajectories. This approach enables a fine-grained gradient flow, allowing more controlled loss calculations that consider events like agent deaths and new agent spawns, shared across all entities. The implementation involves grouping entities into subgroups, maintaining a suitable trajectory buffer, collecting subgroup trajectories, and employing sampling and batching techniques to calculate the policy loss based on the importance sampling and advantage term.

Our method can also improves training efficiency by prioritizing high-advantage or high-return data (quality data), enabling collective behavior learning not achievable with Independent PPO. We thank the reviewer for the valuable comment, and will include Independent PPO as a baseline, noting its challenges with dynamic agent numbers and trajectory lengths, where our approach scales better.

The reviewer asks why the "centralized" model is described as having a "local" field-of-view. By "local," we refer to the size of the convolutional kernel in the shared feature extractor, which processes localized segments of the grid-based environment.

The reviewer mentions that shared-parameter strategies in IPPO allow for batching all agents in a single forward pass. While this is true, our approach calculates advantages, returns, and policy losses for separated entity groups, resulting in better performance, as shown in Figure 6 of the main paper.

The reviewer highlights the specificity of the pixel-to-pixel feature extractor to 2D grid environments. We acknowledge this limitation and agree that generalizing this aspect of the implementation is an important direction for future work. We will clarify this point in the future.

We appreciate the suggestion to include the PPO clipping term for clarity and will revise Figure 1 accordingly.

The reviewer points out potential confusion in interpreting the 'done' signal. The initial 'True' flags indicate agents not yet spawned, while subsequent 'True' flags signify agents that have been removed (e.g., exploded). We will clarify this in the text to address the misunderstanding.

Regarding Figure 5, the reviewer notes confusion between "1 unit groups" and "separate trajectories." The "1 unit groups" refer to global trajectories with all units treated as a single group, while "separate trajectories" treat each unit individually. We acknowledge the ambiguity and will revise the figure caption for clarity.

In summary, we thank the reviewer for their constructive feedback, which we believe will significantly improve the paper. We look forward to addressing these points in the revised version.

--------------------------------------

Reviewer 2:

We thank the reviewer for their detailed feedback and valuable insights. We appreciate their patience in reviewing our work, which they found weaker in some areas. Below, we address each point raised in the review.

The reviewer notes that while the paper focuses on the credit assignment problem, the evaluations primarily examine the impact of individual components within the trajectory separation methodology. This is true, as the design of the experiments aimed to analyze how these components implicitly contribute to efficient credit assignment. Trajectory separation enables semi-decentralized training, combining shared elements with a decentralized credit assignment approach. This framework supports a global sparse reward architecture to reinforce collective behavior while maintaining decentralized agent-level assignments. The ablation studies and component evaluations demonstrate its advantages over fully decentralized approaches.

The reviewer also highlighted the absence of MARL algorithms such as QMIX, VDN, and COMA in the evaluations, despite being mentioned in the paper. We agree with this observation and appreciate the suggestion. These methods were omitted due to space and resource constraints, but we recognize their importance and plan to include them in future evaluations.

Regarding the choice of environments, the reviewer notes that LuxAI is less conventional for MARL benchmarks and that the lack of additional environments is a significant omission. We agree that including multiple environments would strengthen the work. Unfortunately, resource limitations prevented us from conducting further experiments. The LuxAI environment was selected for its alignment with potential funding opportunities tied to its official competition.

Finally, the reviewer points out that the paper does not sufficiently address other MARL challenges, such as sparse or delayed rewards. Due to space constraints, we were unable to provide a detailed explanation of our global reward architecture. Our reward design is semi-sparse and semi-delayed, combining a global structure to reinforce collective behavior with individual assignments. It includes a zero-sum sparse reward for winning, supplemented by hierarchical sub-rewards with a decay factor using a gamma coefficient. Given the complexity of the LuxAI environment and its winning conditions, adopting a fully sparse and delayed reward structure would have significantly slowed down the training process. We appreciate the reviewer highlighting a valuable direction for future research on this topic.

In summary, we thank the reviewer for their constructive feedback, which we believe will significantly improve the paper.

--------------------

We thank the reviewer for their detailed feedback and valuable insights. We appreciate their patience in reviewing our work, which they found weaker in some areas. Below, we address each point raised in the review.

The reviewer notes that while the paper focuses on the credit assignment problem, the evaluations primarily examine the impact of individual components within the trajectory separation methodology. This is true, as the design of the experiments aimed to analyze how these components implicitly contribute to efficient credit assignment. Trajectory separation enables semi-decentralized training, combining shared elements with a decentralized credit assignment approach. This framework supports a global sparse reward architecture to reinforce collective behavior while maintaining decentralized agent-level assignments. The ablation studies and component evaluations demonstrate its advantages over fully decentralized approaches.

The reviewer also highlighted the absence of MARL algorithms such as QMIX, VDN, and COMA in the evaluations, despite being mentioned in the paper. We agree with this observation and appreciate the suggestion. These methods were omitted due to space and resource constraints, but we recognize their importance and plan to include them in future evaluations.

Regarding the choice of environments, the reviewer notes that LuxAI is less conventional for MARL benchmarks and that the lack of additional environments is a significant omission. We agree that including multiple environments would strengthen the work. Unfortunately, resource limitations prevented us from conducting further experiments. The LuxAI environment was selected for its alignment with potential funding opportunities tied to its official competition.

Finally, the reviewer points out that the paper does not sufficiently address other MARL challenges, such as sparse or delayed rewards. Due to space constraints, we were unable to provide a detailed explanation of our global reward architecture. Our reward design is semi-sparse, combining a global structure to reinforce collective behavior with individual assignments. It includes a zero-sum sparse reward for winning, supplemented by hierarchical sub-rewards with a decay factor using a gamma coefficient. We appreciate the reviewer highlighting a valuable direction for future research on this topic.

In summary, we thank the reviewer for their constructive feedback, which we believe will significantly improve the paper.

----

Reviewer 3:

We thank the reviewer for their detailed and supportive feedback and valuable insights. Below, we address each point raised.

The reviewer highlights that the proposed method’s contribution to the credit assignment problem is unclear. To clarify, trajectory separation connects agents through a shared sparse global reward structure and shared policy and value losses, unlike independent PPO, where agents learn separately. Agents have collective losses computed from sub-losses based on batched observation-action pairs from individual trajectories (Figure 1). This method enables fine-grained gradient flow, considering events like agent deaths and spawns.

The reviewer also notes the lack of explanation for the 'hybrid' model mentioned in Section 1. The hybrid approach refers to our end-to-end solution utilizing a semi-decentralized training framework. We acknowledge this oversight and will correct it in the revised version.

Regarding the LuxAI environment, the reviewer points out missing details. Mobile units and factories, both considered agents, change dynamically in number, posing challenges for algorithms like IPPO but addressed by our method. While LuxAI is unconventional as a benchmark, it was chosen due to potential funding opportunities. Appendix E in the supplementary material provides additional details.

The reviewer notes that the rewards architecture requires further elaboration. Our semi-sparse global reward design reinforces collective behavior while assigning rewards individually. It includes a zero-sum sparse reward for winning and hierarchical sub-rewards decaying with a gamma coefficient. The reward architecture is global. We thank the reviewer for the feedback and will improve the explanation in the revised version.

On action queues, the reviewer notes a lack of discussion on delayed actions. This omission is because our method does not utilize action queues. We will clarify this in the future.

The reviewer also highlights the limitation of using a single environment for evaluation. We agree that additional benchmarks are essential and plan to include them in future work alongside standard algorithms like IPPO. 

Finally, the reviewer points out the omission of the main architecture diagram in the main paper. Due to space constraints, it was included in Appendix A of the supplementary material. 

In summary, we thank the reviewer for their constructive feedback, which we believe will significantly improve the paper.