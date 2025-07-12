#Cloud Manufacturing Optimization
A complete multi-objective optimization framework for sustainable service composition in cloud manufacturing environments. This project assigns manufacturing tasks to service providers while optimizing environmental sustainability, economic performance, social impact, execution time, cost, and quality.

Overview
This framework addresses the assignment of manufacturing tasks to service providers in a cloud manufacturing environment by optimizing across the following dimensions:

Environmental sustainability

Economic performance

Social impact

Execution time

Total cost

Service quality

Logistics cost

Problem Statement
Given:

A set of manufacturing activities

A pool of service providers

The goal is to determine an optimal assignment of tasks such that:

Sustainability thresholds (environmental, economic, social) are met or exceeded

Requester-level constraints on time, cost, and quality are satisfied

Overall system performance is optimized across all objectives

Model Components
Decision Variables
Binary variables representing assignment of activities to providers

Auxiliary binary variables for linking dependencies between tasks

Total execution time, cost, and service quality per requester

Objectives
Minimize environmental, economic, and social costs

Maximize overall service quality

Minimize logistics cost

Constraints
Sustainability thresholds must be met for each activity

Time, cost, and quality constraints per requester

Each activity assigned to exactly one provider

Auxiliary constraints ensure logical consistency in service linking

Meta-Goal Programming Reformulation
The multi-objective problem is reformulated into a goal programming model using deviation variables.

Methods
Weighted Method: minimize weighted sum of deviations from targets

Mini-Max Method: minimize the worst-case (maximum) deviation

Meta-Goal Types
Type I: soft constraints with maximum aggregated deviation

Type II: individual deviations capped by control parameters

Type III: limit on number of goals allowed to deviate using binary indicators

NSGA-II Heuristic for Large-Scale Optimization
For complex or large-scale problems, the NSGA-II (Non-dominated Sorting Genetic Algorithm II) heuristic is used, featuring:

Non-dominated sorting of solutions

Crowding distance calculation for diversity preservation

Tournament-based parent selection

Simulated Binary Crossover (SBX)

Polynomial mutation

Population merging and truncation to retain best solutions

NSGA-II Parameters
Crossover probability

Mutation probability

Population size

Maximum number of iterations

Parameters are tuned using the Taguchi method to optimize performance.
