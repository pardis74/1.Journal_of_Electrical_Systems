\documentclass[11pt]{article}
\usepackage[margin=1in]{geometry}
\usepackage{amsmath, amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{enumitem}
\setlength{\parskip}{0.8em}
\setlength{\parindent}{0pt}

\title{Sustainable Service Composition in Cloud Manufacturing}
\author{Ali Vaziri, Pardis Sadatian Moghaddam, Anita Ershadi Oskouei}
\date{}

\begin{document}

\maketitle

\section*{Overview}

This repository provides a complete multi-objective optimization framework for service composition in cloud manufacturing environments. The objective is to assign manufacturing tasks (activities) to service providers while optimizing for:

\begin{enumerate}
    \item Environmental Sustainability
    \item Economic Performance
    \item Social Impact
    \item Execution Time
    \item Total Cost
    \item Service Quality
    \item Logistics Cost
\end{enumerate}

\section*{Problem Statement}

Given a set of activities $i$ and service providers $j$, the model determines the optimal assignment such that:

\begin{enumerate}
    \item Sustainability components (environmental, economic, and social) meet or exceed defined thresholds.
    \item Execution time, cost, and service quality for each requester $d$ remain within acceptable limits.
    \item Overall system performance across sustainability and logistics dimensions is optimized.
\end{enumerate}

\section*{Mathematical Model}

\subsection*{Decision Variables}
\begin{enumerate}
    \item $x_{ij} \in \{0,1\}$: 1 if activity $i$ is assigned to provider $j$
    \item $z_{iji'j'} \in \{0,1\}$: Auxiliary binary linking variable
    \item $t_d, p_d, q_d \geq 0$: Total time, cost, and quality for requester $d$
\end{enumerate}

\subsection*{Objective Functions}

\begin{align*}
    \text{OBJ}_1 &= \min \sum_{i,j} \left( EnCS_{ij} + EcCS_{ij} + sCS_{ij} \right) x_{ij} \\
    \text{OBJ}_2 &= \max \left[ QoS_d = \min(t_d, p_d, \frac{1}{q_d}) \right] \\
    \text{OBJ}_3 &= \min \sum_{i,j,i',j'} plc_{iji'j'} \cdot z_{iji'j'}
\end{align*}

\subsection*{Constraints}

\textbf{Sustainability Thresholds:}
\begin{align*}
    \sum_j EnCS_{ij} x_{ij} &\geq En_{\min} \quad \forall i \\
    \sum_j EcCS_{ij} x_{ij} &\geq Ec_{\min} \quad \forall i \\
    \sum_j sCS_{ij} x_{ij} &\geq Sp_{\min} \quad \forall i
\end{align*}

\textbf{Time, Cost, and Quality:}
\begin{align*}
    t_d &= \sum_{i,j} tmcs_{ij} x_{ij} + \sum_{i,i',j,j'} ttcs_{iji'j'} z_{iji'j'} \leq t_d^{\max} \\
    p_d &= \sum_{i,j} pmcs_{ij} x_{ij} + \sum_{i,i',j,j'} ptcs_{iji'j'} z_{iji'j'} \leq p_d^{\max} \\
    q_d &\geq q_d^{\min}
\end{align*}

\textbf{Assignment Constraint:}
\[
    \sum_j x_{ij} = 1 \quad \forall i
\]

\textbf{Linearization Constraints:}
\begin{align*}
    z_{iji'j'} &\leq \frac{x_{ij} + x_{i'j'}}{2} \\
    z_{iji'j'} &\geq x_{ij} + x_{i'j'} - 1
\end{align*}

\textbf{Non-Negativity:}
\[
    t_d, p_d, q_d \geq 0; \quad x_{ij}, z_{iji'j'} \in \{0,1\}
\]

\section*{Meta-Goal Programming}

The multi-objective model is reformulated using meta-goal programming with deviation variables $n_i$ and $p_i$:

\[
    f_i(x) + n_i - p_i = t_i, \quad \forall i
\]

\subsection*{Weighted Method}
\[
    \min \sum_{i=1}^{s} \omega_i \frac{n_i}{r_i}
\]

\subsection*{Mini-Max Method}
\[
    \min \max_{i=1}^{s} \left( \omega_i \frac{n_i}{r_i} \right)
\]

\subsection*{Meta-Goal Types}
\begin{enumerate}
    \item Type I: $\sum_{i \in S_k^1} \omega_i \frac{n_i}{r_i} \leq Q_k^1$
    \item Type II: $\omega_l \frac{n_i}{r_l} - D \leq 0$, $D \leq Q_l^2$
    \item Type III: $n_i - M_i y_i \leq 0$, $\frac{\sum_i y_i}{|S_r^3|} \leq Q_r^3$, $y_i \in \{0,1\}$
\end{enumerate}

\section*{NSGA-II Heuristic (for Large-Scale Problems)}

For high-dimensional instances, NSGA-II is applied using:

\begin{enumerate}
    \item Non-dominated sorting
    \item Crowding distance calculation
    \item Tournament selection
    \item Simulated Binary Crossover (SBX)
    \item Polynomial mutation
    \item Population merging and truncation
\end{enumerate}

\textbf{Parameters:}
\begin{enumerate}
    \item $P_c$: Crossover probability
    \item $P_m$: Mutation probability
    \item $N_p$: Population size
    \item Max iterations
\end{enumerate}

Parameters are tuned via the Taguchi method.

\section*{Repository Structure}

\begin{verbatim}
.
├── data/                   # Input datasets
├── models/                 # Optimization models
│   ├── goal_programming.py
│   └── nsga2_optimizer.py
├── utils/                  # Pre/postprocessing utilities
├── main.py                 # Main execution script
├── results/                # Output and Pareto fronts
└── README.md               # Project description
\end{verbatim}

\section*{Usage Instructions}

1. Clone the Repository:
\begin{verbatim}
git clone https://github.com/yourusername/cloud-manufacturing-optimization.git
cd cloud-manufacturing-optimization
\end{verbatim}

2. Install Dependencies:
\begin{verbatim}
pip install -r requirements.txt
\end{verbatim}

3. Run the Model:
\begin{verbatim}
# Meta-Goal Programming:
python main.py --mode goal

# NSGA-II Optimization:
python main.py --mode nsga2
\end{verbatim}

\section*{Outputs}
\begin{enumerate}
    \item Pareto-optimal service assignments
    \item Sustainability score distributions
    \item Time-cost-quality trade-offs
    \item Logistics cost visualizations
\end{enumerate}

\section*{Citation}
Ali Vaziri, Pardis Sadatian Moghaddam, Anita Ershadi Oskouei, \textit{Development of Service Compositions in Cloud Manufacturing Processes Based on System Sustainability Components}, 2025.

\section*{Contact}

For questions or collaboration:

\begin{enumerate}
    \item Pardis Sadatian: \texttt{pardis.email@example.com}
    \item Ali Vaziri: \texttt{ali.email@example.com}
\end{enumerate}

\end{document}
