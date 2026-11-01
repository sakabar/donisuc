**English** | [日本語](motivation.ja.md)

# Motivation and Directions for Improvement

This document records the issues identified through using and improving Schronu, along with what we want to achieve with Donisuc. These directions for improvement are a starting point for design discussions; specific implementation approaches and timelines have not yet been decided. “Design considerations” lists topics that need further clarification and examination.

## Rethink the Concepts and Implementation Structure

**Problem**: Features have been added incrementally during use, with important concepts introduced later. As a result, the relationships between concepts and the implementation structure have become complex. The theoretical foundations have also not been sufficiently clarified, leaving it unclear what guarantees can be made about the schedule.

**Desired outcome**: Revisit the necessary concepts and their relationships from the foundations of the design, so that we can explain the rules that determine the schedule and the properties those rules can guarantee.

**Design considerations**: Identify the concepts needed across the issues below and consider which data structures and rules should represent them.

## Treat Future Projections as a Core Concept

**Problem**: The “projection” feature, which displays recurring tasks indefinitely into the future, was added later. Its place in the design needs to be clarified, including how the scheduled times of projected tasks can be adjusted.

**Desired outcome**: Make projections a core concept, allowing users to review future recurring tasks and adjust their scheduled times.

**Design considerations**: Consider how to distinguish projections from confirmed plans, how broadly a change to one occurrence's scheduled time should apply, and how to represent indefinitely recurring tasks.

## Clarify Fixed Plans, Task Segmentation, and Indivisible Plans

**Problem**: Concepts such as plans with fixed times, task segmentation, and atomic plans were introduced later.

**Desired outcome**: Account for these concepts from the beginning of the design and make their scheduling constraints explicit.

**Design considerations**: Clarify which elements are fixed, such as start or end times; the units into which tasks can be split; and the precise meaning and constraints of atomic plans (plans that must be executed without splitting).

## Preserve Recurring Procedures as Templates

**Problem**: A recurring task can regenerate only a single task. Even after a task has been broken down, it must be broken down again for the next occurrence.

**Desired outcome**: Introduce “recurrence templates” that preserve multiple tasks and their relationships, reducing the effort of recreating the same procedure each time.

**Design considerations**: Consider how to separate information held in a template from information held in each occurrence, and how template changes should propagate to individual occurrences.

## Represent Buffers as Part of the Plan

**Problem**: Tasks automatically fill available time, leaving no way to represent buffers. As a result, even delays that could otherwise be absorbed by a buffer without changing the plan require manual task postponement, taking time to make those adjustments.

**Desired outcome**: Represent buffers explicitly as part of the plan, allowing users to decide whether to fill available time with tasks.

**Design considerations**: Consider whether a buffer should be represented as a task or a time block, and how it should be handled when the schedule is recalculated.

## Understand How Delays Affect Future Plans

**Problem**: The schedule must be recalculated each time to determine what will happen when. We cannot clearly pinpoint which future plans are affected by today's delays. For example, even if the plans for each of the next 28 days fit within the time available that day, a recurring task taking longer than estimated may update the estimates for its future occurrences, leaving many days with more work than they can accommodate.

**Desired outcome**: Identify the plans affected by a delay and how they change, so users can decide what to postpone.

**Design considerations**: Use concrete examples to clarify what comparing schedules before and after recalculation can and cannot reveal. Then consider how to represent and present changes to plans and their causes.

## Design Data Structures Around Required Use Cases

**Problem**: The data structures needed to support use cases such as calendar views, search, and viewing tasks across a project have not been sufficiently worked out.

**Desired outcome**: Identify the required features and design data structures that can support them consistently.

**Design considerations**: Consider what information each view and search needs, and how tasks, plans, and projects should be related.

## Handle Sleep Across Day Boundaries Properly

**Problem**: Sleep spanning multiple days is not handled well and cannot be represented as a task. This leads to problems such as not tracking sleep duration and naturally falling into a pattern of postponing sleep.

**Desired outcome**: Include sleep in planning and enable users to decide how to adjust it in relation to other tasks.

**Design considerations**: Consider how to represent plans spanning day boundaries and how to handle the relative priorities of sleep and other tasks.

## Show Required Preparation and the First Step Toward Action

**Problem**: Prerequisites for an action and the first step toward starting it cannot be displayed adequately. An example is preparing a key before entering the house. Even if a task is broken down, only a single task is regenerated for each recurrence, so this information cannot be carried over to the next occurrence.

**Desired outcome**: Show the preparation needed for an action and the first step that lowers the psychological barrier to starting, and make them reusable across occurrences. Here, we call this first step a “trigger.”

**Design considerations**: Clarify the differences between prerequisites, triggers, and ordinary subtasks, and consider how to preserve them in recurrence templates.

## Clarify the Meaning and Guarantees of Priorities

**Problem**: Multiple tasks can have the same priority, making it difficult to make logically precise statements about task ordering.

**Desired outcome**: Make explicit what priorities mean and what they guarantee about execution order and scheduling.

**Design considerations**: First, specify the properties we want to guarantee. Then consider whether to allow equal priorities and, if so, how to determine the order among tasks with the same priority.

## Connect Daily Tasks to Visions and Goals

**Problem**: Schronu does not address choosing a vision or setting goals. As a result, even when tasks are managed, users may struggle to decide how to move their lives or projects in the desired direction, or how to prioritize tasks.

**Desired outcome**: Represent the connections between daily tasks, visions, and goals, helping users decide how to allocate time to the projects they want to advance.

**Design considerations**: Consider the scope of Donisuc's responsibilities, including recording visions and goals, linking them to tasks, and supporting time allocation.

## Organize Functionality Across the CLI, Web, and MCP Interfaces

**Problem**: The CLI, MCP, and Web interfaces were built in that order, and the capabilities available through each interface differ.

**Desired outcome**: Provide all the functionality needed for the intended use cases: everyday use by a person (Web), AI-assisted task creation (MCP), and procedural operations on multiple tasks (CLI).

**Design considerations**: Centralize operations and business rules in the application layer, and expose the operations required for each use case through the corresponding interface.

## Account for Work Not Yet Broken Down as Future Workload

**Problem**: Some plans have an estimated total effort and a deadline, but cannot be broken down into concrete steps until work begins. For example, completing an exam workbook by the day before the exam may be estimated to take around 50 hours, but those 50 hours cannot be scheduled as a single day's task. On the other hand, postponing the work and leaving it out of sight until it can be broken down means the time required before the deadline may be overlooked in planning.

**Desired outcome**: Keep plans that have not yet been broken down into executable units, together with their total effort and deadlines, and include them in projections of future workload. As more becomes known after starting, progressively break down the work and update the remaining effort and plan.

**Design considerations**: Consider how to account for work not yet broken down without committing to specific execution times. Also clarify how to relate the resulting tasks to the remaining work that has not yet been broken down, prevent double counting or omissions, and update remaining effort as work progresses.

## Distinguish Actions from States

**Problem**: Schronu has primarily been used to record executable actions as tasks and measure the time spent on them. However, users may want to start with a desired state, such as “a reply has been received” or “my manager has approved my proposal,” and then determine the actions needed to reach it. Because actions and states are not currently distinguished, it is difficult to treat sending a request separately from receiving a reply and reaching the desired state.

**Desired outcome**: Represent desired states separately from the actions taken to achieve them, while linking the two. For example, completing an explanation or request for approval does not necessarily mean approval has been obtained. Represent that distinction and keep track of items while waiting for replies or approval.

**Design considerations**: Clarify the relationships among current states, desired states or acceptance criteria, and the actions intended to achieve them. Also consider how to determine and record whether a state has been reached, distinguish time spent waiting for someone else from time spent doing work, and use states as prerequisites for subsequent actions.
