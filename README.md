# RoboReflex

**A recovery layer for robot policies: detect failures, retry skills, and replan when needed.**

RoboReflex will supervise an existing vision-language-action policy at action boundaries. It will use structured observations and recent execution feedback to choose whether to continue, observe again, call an available recovery skill, or request a new plan. Jev is the first intended semantic backend.

**Status: design-stage project.** This repository publishes the proposed interface, scope, and evaluation plan. There is no working Jev adapter, VLA integration, simulator run, or measured recovery result yet. The example is an authored fixture.

## First demonstration

Run the same policy with the same disturbance in two matched episodes. Move the target or interrupt a grasp. Compare whether the original policy and the supervised policy complete the task, how long they take, and how often the supervisor intervenes unnecessarily.

The first milestone covers three failure families: empty grasp, object slip, and target relocation. Recovery skills must be supplied by the host integration; RoboReflex does not generate continuous joint commands.

## Proposed interface

```text
Observation + execution history + available recovery skills
    -> semantic decision backend
    -> bounded choice and recorded decision evidence
    -> host policy / recovery skill / replanner
```

The host retains perception, skill execution, high-frequency control, and local constraints. Jev receives text or structured state. A camera-based integration needs a separate perception component, whose errors and latency count toward the system result.

See `examples/observation.json` for a proposed input contract. This is a project-level interface sketch, not a TypeSafe API request.

## Roadmap

- [ ] Implement a small observation, decision, and skill-registration interface.
- [ ] Integrate one existing policy and one simulator; target LIBERO first.
- [ ] Add a rule-based recovery baseline and a Jev backend.
- [ ] Add repeatable disturbances and independent evaluation seeds.
- [ ] Publish requests, decisions, trajectories, failures, and timing.
- [ ] Validate reuse with a second policy before expanding the framework.

## What would count as an improvement?

Compare the unmodified policy, rule recovery, VLM recovery, and Jev recovery with matched information access. Report task success, recovery success, unnecessary intervention rate, total latency, API cost, and compute cost. Select thresholds on separate calibration episodes. Keep privileged simulator observations and visual observations in separate result tables.

Choice confidence is not the probability that an action will succeed. End-to-end recovery gains remain an experimental question.

## 中文说明

RoboReflex 的目标是给已有 VLA 增加失败识别与恢复接口。首版聚焦抓空、滑落、目标移动三个问题，由语义判断选择继续、重新观察、调用已有恢复技能或请求重新规划。

当前为设计阶段，尚无真实模型接入和仿真评测。计划先验证一个策略、一个环境，再检查能否复用到第二个策略。不会把手工编写的示例当成实验结果。

## Related work and sources

- [TypeSafe models](https://docs.typesafe.ai/models)
- [TypeSafe confidence](https://docs.typesafe.ai/confidence)
- [LIBERO](https://github.com/Lifelong-Robot-Learning/LIBERO)
- [EmbodiedJev](https://github.com/FBddcz/embodied-jev): an existing robot decision workbench.
- [jev-libero](https://github.com/Dimweaker/jev-libero): existing Jev control and physics-preview experiments.

Independent community project; not affiliated with TypeSafe or the linked projects. MIT licensed; upstream components retain their licenses.
