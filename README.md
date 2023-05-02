Download Link: https://assignmentchef.com/product/solved-comp550-project-5
<br>
<strong>Algorithmic Robotics</strong>

<h1>Special Topics in Motion Planning</h1>

<strong>The documentation for </strong><strong>OMPL </strong><strong>can be found at </strong><a href="http://ompl.kavrakilab.org/"><strong>http://ompl.kavrakilab.org.</strong></a>

In this assignment, you are free to select a project from one of several below or propose your own interesting motion planning problem. Those who propose a project must receive prior approval from the instructor. This project may be completed in pairs.

<h2>Deliverables</h2>




An initial progress report is due Tuesday November 12th at 1pm on Canvas. This report should be short, no longer than one page in PDF format. At a minimum, the report should state who your partner is, which project you are working on, and what progress you have made thus far. Those who propose their own project or deviate significantly from a proposed project must receive approval from the instructor prior to submitting their progress report. Those completing the project in groups need to provide just one progress report.

Final submissions are due Wednesday November 27th at 1pm. Submissions are on Canvas and consist of two things. First, to submit your project, clean your build space with make clean, zip up the project directory into a file named Project2 &lt;your NetID&gt; &lt;partner’s NetID&gt;.zip. Your code must compile within a modern Linux environment. If your code compiled on the virtual machine, then it will be fine. If you deem it necessary, also include a README with details on compiling and executing your code.

Second, submit a written report that summarizes your experiences and findings from this project. The report should be no longer than 10 pages, in PDF format, and contain (at least) the following information:

<ul>

 <li>A problem statement and a short motivation for why solving this problem is useful.</li>

 <li>The details of your approach and an explanation of how/why this approach solves your problem.</li>

 <li>A description of the experiments you conducted. Be precise about any assumptions you make on the robot or its environment.</li>

 <li>A quantitative and qualitative analysis of your approach using the experiments you conduct.</li>

 <li>Rate the difficulty of each exercise on a scale of 1–10 (1 being trivial, 10 being impossible). Give an estimate of how many hours you spent on each exercise, and detail what was the hardest part of the assignment.</li>

</ul>

Please submit the PDF write-up separate from the source code archive. Those completing the project in groups need to provide just one submission for code and report. In addition to the code and report, there will also be a short (∼5 minute) in-class presentation on your chosen topic. Details of the presentation will be announced closer to the presentation dates.

If your code or your report is missing by the deadline above, your project is late. The latest date for submission is Friday at 5pm before the last week of classes. Your presentation is during the last week of classes.

<h2>Project Topics</h2>

The topics proposed below are a broad spectrum of interesting and useful problems within robot motion planning. Each topic comes with its own set of questions and deliverables that should be addressed in the code and written report. For topics marked 450-only, you may only choose the topic if both you and your partner are enrolled in the undergraduate versions of the class. The graduate section of the topics is required if either you or your partner is enrolled in the graduate versions of the course, and optional if you are both enrolled in the undergraduate versions. No extra credit will be rewarded for undergraduate students completing the graduate requirement. Although it may not be stated explicitly, <em>visualization is essential </em>for virtually every project, report, and presentation to establish correctness of the implementation.

<h2>5.1 Path Clustering (450-Only)</h2>

Sampling-based motion planning algorithms can produce many different paths for a given query. However, many of these paths can be virtually identical to one another. Whether this is the case is often difficult to determine, particularly if the configuration space has more than 3 dimensions since it is difficult to visualize such a space. In this project you will implement a distance metric for <em>paths </em>using a given distance metric for <em>configurations</em>. The path distance metric is then used to cluster a collection of paths that begin and end at the same configurations.

Path Distance:

The distance from a path A to a path B can be defined by integrating the distance to the closest point on B for a point moving along A. This formulation, however, is problematic since two very different paths can still have distance 0 (see Figure 1(a) for an example).

(a) Path distance should be sig-          (b) Increasingly complex environments which result in increasingly many path clusters.

nificantly greater than 0 between the solid and dotted <sub>curve. </sub>Figure 1: Path distance and path clustering.

A more sophisticated approach first constructs a monotonic mapping between curve-length parametrizations of A and B and then integrates the distance between corresponding points on A and B. Under the assumption that paths A and B are densely and uniformly sampled so that there are <em>m </em>states along A and <em>n </em>states along <em>B</em>, we can compute the distance using <em>dynamic programming</em>. Let <em>D<sub>i</sub></em><em><sub>,j </sub></em>be the distance between state <em>i </em>∈ <em>A </em>and state <em>j </em>∈ <em>B </em>using the standard distance metric for configurations. By taking advantage of the optimal substructure of this computation, we can compute the distance between the two paths by finding a minimum-distance mapping between the states in A and B. Let <em>C<sub>i</sub></em><em><sub>,j </sub></em>denote the minimum distance mapping up to state <em>i </em>∈ <em>A </em>and state <em>j </em>∈ <em>B</em>. Then <em>C<sub>m</sub></em><em><sub>,n </sub></em>is the total distance between two paths, where <em>C</em><sub>1</sub><em><sub>,</sub></em><sub>1 </sub>= <em>D</em><sub>1</sub><em><sub>,</sub></em><sub>1 </sub>and

<em>C<sub>i</sub></em><em>,</em><em><sub>j </sub></em>= min[<em>C<sub>i</sub></em>−1<em>,</em><em><sub>j</sub></em><em>,C<sub>i</sub></em><em>,</em><em><sub>j</sub></em>−1]+<em>D<sub>i</sub></em><em>,</em><em><sub>j</sub></em><em>.</em>

Path Clustering:

Once you have implemented a distance metric for paths, you can begin clustering paths that are <em>close </em>given your metric. You may choose any clustering algorithm: single-linkage (nearest neighbor) clustering, <em>k</em>-means, or more sophisticated techniques. Single-linkage and <em>k</em>-means are simple, heuristic approaches that attempt to group paths together into a set of clusters. Given the clustering output, one can visually assess the cluster quality using a <em>similarity </em>matrix. The similarity matrix is computed by sorting the data based on cluster id and then visualizing the elements of <em>D </em>(after normalization). An example of a good clustering and similarity matrix is shown in Figure 2.

If two paths are considered close according to the distance metric, but cannot be continuously deformed from one to another without passing through an obstacle, then these paths are topologically different. Ideally,

Figure 2: (Left) An example data set and grouping into three clusters. (Right) The similarity matrix for the clustering. Red squares along the diagonal indicate a good clustering.

we would like paths in different homotopy classes to end up in different clusters. This is a hard problem in general, but the following heuristic can potentially improve the clustering: if the straight-line path between <em>a<sub>i </sub></em>∈ <em>A </em>to <em>b<sub>j </sub></em>∈ <em>B </em>is not valid, then <em>D<sub>i</sub></em><em><sub>,j </sub></em>should change to a large value to discourage clustering paths in different homotopic classes. If all straight line paths between every <em>a<sub>i </sub></em>and corresponding <em>b<sub>j </sub></em>are valid, the original distance is returned.

Deliverables:

Implement the path distance function, with and without the homotopy check, and a clustering algorithm as described above. The implementation should work for any arbitrary, user-defined distance metric. Verify that the distance function reports a significant different between the two paths in Figure 1a. Evaluate your implementation in a variety of environments, like those in Figure 1b, to verify whether your clusters make sense. You may assume a point robot translating in R<sup>2</sup>.

How well does the path distance function and clustering algorithm classify your data? Does the homotopy heuristic improve the clustering? How did you select the number of clusters? Be sure to cluster both small and large numbers of paths, and to visualize the similarity matrix of your clustering when presenting your conclusions.

Protips:

Note that even in the simplest case with just one obstacle, there exist more than two clusters of paths (e.g., there is a cluster of paths that all go around the obstacle clockwise 5 times before reaching the goal). Generally speaking, there are infinitely many homotopy classes, thus infinitely many “ideal” clusterings.

To find many different paths, you must run a planner many times and extract a solution path from each run. It may be advantageous to generate paths from different planners for diversity. Moreover, you may also want to disable path simplification for some or all of the paths that you generate.

Bonus (5 pt):

Clustering algorithms can produce wildly different results on the same input. As a bonus experiment, compare and contrast two clustering algorithms on paths using the path distance function described above. You can choose any pair of clustering algorithms: single linkage (nearest neighbor) clustering, <em>k</em>-means, etc. Present a qualitative review of the clustering algorithms you choose. What are the strengths and weaknesses of the algorithms? Compare the clusterings computed by the algorithms.

<h2>5.2 Planning Under Uncertainty (450-Only)</h2>

For physical robots, actuation is often imperfect: motors cannot instantaneously achieve a desired torque, the execution time to apply a set of inputs cannot be hit exactly, or wheels may unpredictably slip on loose terrain just to name a few examples. We can model the actuation error for many situations using a conditional probability distribution, where the probability of reaching a state <em>q</em><sup>0 </sup>depends on the initial state <em>q</em>

and the input <em>u</em>, written succinctly as <em>P</em>(<em>q</em><sup>0</sup>|<em>q</em><em>,u</em>). Figure 3: Even under an optimal policy, a system with actuation uncertainty can fail to reach the goal (green circle) consistently.

Because we cannot anticipate the exact state of

<em>From Alterovitz et al.; see below.</em>

the robot <em>a priori</em>, algorithms that reason over uncertainty compute a <em>policy </em>instead of a path or trajectory since the robot is unlikely to follow a single path exactly. A policy is a mapping of every state of the system to an action. Formally, a policy <em>π </em>is a function <em>π </em>: <em>Q </em>→ <em>U</em>, where <em>Q </em>is the configuration space and <em>U </em>is the control space. When we know the conditional probability distribution, an optimal policy to navigate the robot can be computed efficiently by solving a Markov decision process.

Markov decision process (MDP):

When the transitional probability distribution depends only on the current state, the robot is said to have the

Markov property. For Markov systems, an optimal policy is obtained by solving a Markov decision process (MDP). An MDP is a tuple (<em>Q</em><em>,U</em><em>,P</em><em>,R</em>), where:

<ul>

 <li><em>Q </em>is a set of states</li>

 <li><em>U </em>is a set of controls applied at each state</li>

 <li><em>P </em>: <em>Q</em>×<em>U </em>×<em>Q </em>→ [0<em>,</em>1] is the probability of reaching <em>q</em><sup>0 </sup>∈ <em>Q </em>via initial state <em>q </em>∈ <em>Q </em>and action <em>u </em>∈ <em>U</em>.</li>

 <li><em>R </em>: <em>Q </em>→ R is the reward for reaching a state <em>q </em>∈ <em>Q</em>.</li>

</ul>

An optimal policy over an MDP maximizes the total <em>expected reward </em>the system receives when executing a policy. To compute an optimal policy, we utilize a classical method from dynamic programming to estimate the expected total reward for each state: Bellman’s equation. Let <em>V</em><sub>0</sub>(<em>q</em>) be an arbitrary initial estimate for the maximum expected value for <em>q</em>, say 0. We iteratively update our estimate for each state until we converge to a fixed point in our estimate for all states. This fixed-point algorithm is known as <em>value iteration </em>and can be succinctly written as

<em>V<sub>t</sub></em><sub>+1</sub>(<em>q</em>) = <em>R</em>(<em>q</em>)+max<sup>∑</sup><em>P</em>(<em>q</em><sup>0</sup>|<em>q</em><em>,u</em>)<em>V<sub>t</sub></em>(<em>q</em><sup>0</sup>)<em>,                                                               </em>(1)

<em><sub>u</sub></em><sub>∈<em>U </em></sub><em>q</em>0

We stop value iteration when <em>V<sub>t</sub></em><sub>+1</sub>(<em>q</em>) = <em>V<sub>t</sub></em>(<em>q</em>) for all <em>q </em>∈ <em>Q</em>. Upon convergence, the optimal policy for each state <em>q </em>is the action <em>u </em>that maximizes the sum in the equation above.

Sampling-based MDP:

Efficient methods to solve an MDP assume that the state and control spaces are discrete. Unfortunately, a robot’s configuration and control spaces are usually continuous, preventing us from computing the optimal policy exactly. Nevertheless, we can leverage sampling-based methods to build an MDP that approximates the configuration and control space. The <em>stochastic motion roadmap </em>(SMR) is one method to approximate a continuous MDP. Building an SMR is similar to building a PRM; there is a learning phase and a query phase. During the learning phase, states are sampled uniformly at random and the probabilistic transition between states are discovered. During the query phase, an optimal policy over the roadmap is constructed to bring the system to a goal state with maximum probability.

<em>Learning phase: </em>During the learning phase, an MDP approximation of the evolution of a robot with uncertain actuation is constructed. First, <em>n </em>valid states are sampled uniformly at random. Second, the transition probabilities between states are discovered. SMR uses a small set of predefined controls for the robot. The transition probabilities can be estimated by simulating the system <em>m </em>times for each state-action pair. Note that <em>m </em>has to be large enough to be an accurate approximation of the transition probabilities. For simplicity,

we assume that the uncertainty is represented with a Gaussian distribution. Once a resulting state is generated from the state-action pair, you can match it with the <em>n </em>sampled states through finding its nearest neighbor, or if a sampled state can be found within a radius. It is also useful to add an <em>obstacle </em>state to the SMR to indicate transitions with a non-zero probability of colliding with an obstacle.

<em>Query phase: </em>The SMR that is constructed during the learning phase is used to extract an optimal policy for the robot. For a given goal state (or region), an optimal policy is computed over the SMR that maximizes the probability of success. To maximize the probability of success, simply use the value iteration algorithm from equation 1 with the following reward function:

(

<table width="207">

 <tbody>

  <tr>

   <td width="79">0 <em>R</em>(<em>q</em>) =</td>

   <td width="128">if <em>q </em>is not a goal state<em>,</em></td>

  </tr>

  <tr>

   <td width="79">1</td>

   <td width="128">if <em>q </em>is a goal state<em>.</em></td>

  </tr>

 </tbody>

</table>

Note: it is assumed that the robot stops when it reaches a goal or obstacle state. In other words, there are no controls for the goal and obstacle states and <em>V<sub>t</sub></em>(<em>q</em>) = <em>R</em>(<em>q</em>) is constant for these states.

Deliverables:

Implement the SMR algorithm for motion planning under action uncertainty with a 2D steerable needle. Modeling a steerable needle is similar to a Dubins car; see section III of the SMR paper (cited below) for details on the exact configuration space and control set. Construct some interesting 2D environments and visualize the execution of the robot under your optimal policy. Simulated execution of the steerable needle is identical to the simulation performed when constructing the transition probabilities. Simulate your policy many times. Does the system always follow the same path? Does it always reach the goal? How sensitive is probability of success to the standard deviation of the Gaussian noise distribution?

Reference:

<ol start="2007">

 <li>Alterovitz, T. Simeon, and K. Goldberg, The Stochastic Motion Roadmap: A Sampling Framework for´ Planning with Markov Motion Uncertainty. In <em>Robotics: Science and Systems</em>, pp. 233–241, 2007. <a href="http://goldberg.berkeley.edu/pubs/rss-Alterovitz2007_RSS.pdf">http://goldberg.berkeley.edu/pubs/rss-Alterovitz2007</a> <a href="http://goldberg.berkeley.edu/pubs/rss-Alterovitz2007_RSS.pdf">RSS.pdf</a><a href="http://goldberg.berkeley.edu/pubs/rss-Alterovitz2007_RSS.pdf">.</a></li>

</ol>

Bonus (5 pt):

The value <em>V</em>(<em>q</em>) computed by value iteration for each state <em>q </em>is the estimated probability of reaching the goal state under the optimal policy of the SMR starting at <em>q</em>. As a bonus experiment, we can check how accurate this probability is. Keeping the environment, start, and goal fixed, generate 20 (or more) SMR structures with <em>n </em>sampled states and compute the probability of reaching the goal from the start state using value iteration. Then simulate the system many times (1000 or more) under the same optimal policies. How does the difference between the expected and actual probability of success change as <em>n </em>increases? Evaluate starting with a small <em>n </em>(around 1000), increasing until some sufficiently large <em>n</em>; a logarithmic scale for <em>n </em>may be necessary to show a statistical trend.




<h2>5.3 Elastic Kinematic Chains</h2>

Let an elastic kinematic chain be defined as a planar kinematic chain with <em>n </em>revolute joints, where each joint has an angular spring. The spring energy of the chain is ∑<em><sub>i </sub>u</em><sup>2</sup><em><sub>i </sub></em>, where <em>u<sub>i </sub></em>corresponds to joint angle <em>i</em>. Suppose the base link is fixed at (0<em>,</em>0) with orientation 0. Now imagine that the end effector is slowly moved around to different positions and orientations so that the arm is always at a stable equilibrium configuration. How many parameters would you need to describe <em>all </em>stable configurations for <em>n </em>joints? The surprising answer is 3 for any <em>n</em>! There is a relatively straightforward algorithm to determine which configurations are stable (although understanding its correctness is far from trivial).

Figure 4: A sequence of stable configurations for a chain with 4 joints. <em>From McCarthy &amp; Bretl; see below.</em>




There is one small typo in the algorithm in the reference below. The line that reads

<em>P<sub>n</sub></em>−<sub>4 </sub>= (<em>A</em><sup>†</sup><em>B</em>)<em><sup>T</sup></em>(<em>I </em>−<em>NK</em>)<em><sup>T</sup>M</em>(<em>I </em>−<em>NK</em>)(<em>A</em><sup>†</sup><em>B</em>)

should be changed to

<em>P<sub>n</sub></em>−<sub>4 </sub>= <em>Q<sub>n</sub></em>−<sub>4</sub>+(<em>A</em><sup>†</sup><em>B</em>)<em><sup>T</sup></em>(<em>I </em>−<em>NK</em>)<em><sup>T</sup>M</em>(<em>I </em>−<em>NK</em>)(<em>A</em><sup>†</sup><em>B</em>)<em>.</em>

As you can tell from this one line, you will want to use a linear algebra package such as Eigen (a C++ header-only library) or Numpy (a Python library with many Matlab-like functions) for your implementation.

Reference:

<ol start="2012">

 <li>McCarthy and T. Bretl, Mechanics and Manipulation of Planar Elastic Kinematic Chains, in <em>IEEE Intl. Conf. on Robotics and Automation, </em>2012. <a href="http://ieeexplore.ieee.org/xpl/freeabs_all.jsp?arnumber=6224693">http://ieeexplore.ieee.org/xpl/freeabs</a> <a href="http://ieeexplore.ieee.org/xpl/freeabs_all.jsp?arnumber=6224693">all.jsp?arnumber=6224693</a></li>

</ol>

Deliverables: Implement the isStable function from the reference above and use it as a StateValidityChecker to plan paths between stable configurations of an elastic kinematic chain. Add an option to check whether the joint angles (denoted by <em>u<sub>i </sub></em>in the paper) are within −<em>π </em>and <em>π</em>. Then use any OMPL planner to find a path of stable configurations between any pair of stable configurations. Next, write an algorithm that using this function finds a pair of configurations that requires a long path (in terms of number of states on the path). It is assumed that you will use path simplification on each path produced by the planner so that a long path is not simply the result of an unlucky run of the planner. How do the [−<em>π</em><em>,π</em>] joint limits change the results?

Picking reasonable bounds for the 3-parameter space of stable configurations is not entirely obvious. For a 10-joint arm you could use the following bounds on the configuration space: [−15<em>,</em>15]×[−15<em>,</em>15]×[−<em>π</em><em>,π</em>]. They may have to be adjusted for a different number of joints. Implementing the bonus below can help you visualize when the free space becomes almost fractal-like (in which case, you would need to specify a very small value for setStateValidityCheckingResolution). Consider the symmetries that should exist in your c-space obstacles. If your visualization does not match the expected symmetries, there may be an error in your computations.

Graduate: Write code that visualizes the free space of the three-parameter configuration space. This can be done by creating plots of 2D slices of the configuration space, as was done in the reference above. You don’t actually have to compute the boundaries of the configuration space obstacles. It is sufficient to create a dense 3D grid of the configuration space, where for each grid point you compute the state validity. You can then plot each slice of the grid by drawing invalid configurations as black pixels and free configurations as white pixels. If you are using the Python numpy module or Matlab, you can use the spy function.




<h2>5.4 Centralized Multi-robot Planning</h2>

Planning motions for multiple robots that operate in the same environment is a challenging problem. One method for solving this problem is to compute a plan for all of the robots simultaneously by treating all of the robots as a single composite system with many degrees of freedom. This is known as centralized multi-robot planning. A naive approach to solving this problem constructs a PRM for each robot individually, and then plans a path using a typical graph search in the composite PRM (the product of each PRM). Unfortunately, composite PRM becomes prohibitively expensive to store, let alone search. If there are <em>k </em>robots, each with a PRM of <em>n </em>nodes, the composite PRM has <em>n<sup>k </sup></em>vertices!

<table width="654">

 <tbody>

  <tr>

   <td width="627">Searching the exponentially large composite roadmap for a valid multi-robot path is a significant computational challenge. Recent work to solve this problem suggests <em>implicitly </em>searching the composite roadmap using a discrete version of the RRT algorithm (dRRT). At its core, dRRT grows a tree over the (implicit) composite roadmap, rooted at the start state, with the objective of connecting the start state to the goal state.The key steps of dRRT are as follows:1.    Sample a (composite) configuration <em>q<sub>rand </sub></em>uniformly at random.2.    Find the state <em>q<sub>near </sub></em>in the dRRT nearest to the random sample.3.    Using an <em>expansion oracle</em>, find the state <em>q<sub>new </sub></em>in the composite roadmap that is connected to <em>q<sub>near </sub></em>in the closest direction of <em>q<sub>rand</sub></em>. (Figure 6).4.    Add the path from <em>q</em><em>near </em>to <em>q</em><em>new </em>to the tree if it is collision free.        Figure 6: The dRRT expansion step.5.    Repeat 1-4 until until the goal is successfully added to the tree.      <em><sub>From Solovey et al.; see below.</sub></em>Deliverables:Implement the dRRT algorithm for multi-robot motion planning. For <em>n </em>robots, you will first need to generate <em>n</em>PRMs, one for each robot. If your robots are all the same, one PRM will suffice for every robot. Then use your dRRT algorithm to implicitly search the product of the <em>n </em>PRMs. The challenges here are the implementation of the expansion oracle (step 3) and the local planner to check for robot-robot collisions (step 4). The reference below gives details on one possible expansion oracle (section 3.1) and local planner (section 4.2). Evaluate your planner in scenarios with <em>at least </em>four robots, like those in Figure 5. For simplicity you may assume that the robots are planar rigid bodies. You may need to construct additional scenarios to evaluate different properties of the dRRT algorithm.Address the following in your report: Is your dRRT implementation always able to find a solution? How does the algorithm scale as the number of robots increases? Elaborate on the relationship between the quality of the single robot PRMs and the time required to run dRRT. What do the final solution paths look like?Reference:K. Solovey, O. Salzman, and D. Halperin, Finding a needle in an exponential haystack: Discrete RRT for exploration of implicit roadmaps in multi-robot motion planning. In <em>Algorithmic Foundations of Robotics</em>, 2014. <a href="http://robot.cmpe.boun.edu.tr/wafr2014/papers/paper_20.pdf">http://robot.cmpe.boun.edu.tr/wafr2014/papers/paper</a> <a href="http://robot.cmpe.boun.edu.tr/wafr2014/papers/paper_20.pdf">20.pdf</a><a href="http://robot.cmpe.boun.edu.tr/wafr2014/papers/paper_20.pdf">.</a></td>

  </tr>

 </tbody>

</table>

Figure 5: Two challenging multi-robot problems. <em>Left: </em>robots 1 and 2 need to swap positions with robot 4 and 3, respectively. <em>Right: </em>robot <em>i</em><em>,</em><em>i </em>= 1<em>,…,</em>4<em>, </em>needs to swap positions with robot <em>i</em>+4.

<h2>5.5 Decentralized Multi-robot Coordination</h2>

Planning motions for multiple robots simultaneously is a difficult computational challenge. Algorithms that provide hard guarantees for this problem usually require a central representation of the problem, but the size and the dimension of the search space limits these approaches to no more than a handful of robots. Decentralization or distributed coordination of multiple robots, on the other hand, is an effective (heuristic) approach that performs well in practical instances by limiting the scope of information reasoned over at any given time. There exist many effective methods in the literature to practically coordinate the motions of hundreds or even thousands of moving agents.

Figure 7: Two challenging multi-robot problems. <em>Left: </em>robots 1 and 2 need to swap positions with robot 4 and 3, respectively. <em>Right: </em>robot <em>i</em><em>,</em><em>i </em>= 1<em>,…,</em>4<em>, </em>needs to swap positions with robot <em>i</em>+4.




One popular decentralized approach is to assign a priority to each robot and compute motion plans starting with the highest priority. The robot with the highest priority ignores the position of all other robots. Motion plans for subsequent (lower priority) robots must respect the paths for the previously planned (higher priority) robots. In short, the higher priority robots become moving obstacles that lower priority robots must avoid. An example of such an approach is detailed in <em>Berg et al. 2005</em>, cited below.

A more recent approach reasons over the relative velocities between robots, employing the concept of a <em>reciprocal velocity obstacle </em>to simultaneously select velocities for each robot that both avoid collision with other robots and allow each robot to progress toward its goal. This technique is used not only in robotics but also in animation and even video games (e.g., Warhammer 40k: Space Marine, Crysis 2). Briefly, a reciprocal velocity obstacle <em>RVO<sup>B</sup><sub>A </sub></em>defines the space of velocities of robot <em>A </em>that will cause <em>A </em>to collide with another robot <em>B </em>at some point

in the future, given the current position and velocity of <em>A </em>and <em>B</em>. Using Figure 8: The reciprocal velocity obstathis concept, a real-time coordination algorithm can be developed that cle formed by robot <em>B</em>’s current position

and velocity relative to robot <em>A</em>. <em>From </em>simulates the robots for some short time period, then recalculates the

<em>Berg et al. 2008; see below.</em>

velocities for each robot by finding a velocity that lies outside the union of each robot’s <em>RVO</em>.

Deliverables:

Implement one of the methods above for decentralized multi-robot coordination and evaluate the method in multiple scenarios with varying numbers of robots. Graduate students must implement reciprocal velocity obstacles. You may assume that the robots are disks that translate in the plane.

Address the following: Is your method always successful in finding valid paths? Expound upon the kinds of environments and/or robot configurations that may be easy or difficult for your approach? How well does your method scale as you increase the number of robots? Qualitatively, how do the solution paths look? Depending on the method you chose, answer the following questions.

<em>Prioritized method: </em>Compare and contrast the paths for higher priority robots and lower priority robots. How sensitive is your method to the assignment of priorities?

<em>RVO method: </em>Discuss the challenges faced by the <em>RVO </em>approach as the number of robots increases. Is there always a velocity that lies outside of the <em>RVO</em>? How practical do you think this algorithm is?

Protips:

For the prioritized method, the key challenge is the implementation of the prioritized state validity checker. Not only must this function check that the robot being planned does not collide with obstacles, but also for collisions with the higher priority robots that have been previously planned. The notion of time must be incorporated into the configuration space (and possibly the planner as well) to ensure collision-free coordination. A modified version of RRT, EST, or similar planner to generate the single-robot paths is sufficient.

The implementation of the <em>RVO </em>method requires reasoning over the velocities of your robots. You can greatly reduce the complexity of the implementation by planning geometrically and bounding the velocity by limiting the distance a robot can travel during any one simulation step. Moreover, the <em>RVO </em>implementation requires reasoning over the robot geometry; choose simple geometries (circles) and ensure your implementation is correct before considering more complex geometries.

Visualization, particularly animation, is key in debugging these methods. Matlab, Matplotlib, and related packages support generating videos from individual frames.

References:

<ol start="2005">

 <li>van den Berg and M.H. Overmars, Prioritized Motion Planning for Multiple Robots. In <em>IEEE/RSJ Int’l Conference on Intelligent Robots and Systems</em>, pp. 2217-2222, 2005. <a href="http://arl.cs.utah.edu/pubs/IROS2005-2.pdf">http://arl.cs.utah.edu/pubs/ </a><a href="http://arl.cs.utah.edu/pubs/IROS2005-2.pdf">IROS2005-2.pdf</a><a href="http://arl.cs.utah.edu/pubs/IROS2005-2.pdf">.</a></li>

 <li>van den Berg, M. Lin, and D. Manocha, Reciprocal Velocity Obstacles for Real-Time Multi-Agent Navigation. In <em>IEEE Int’l Conference on Robotics and Automation</em>, pp. 1928-1935, 2008. <a href="https://www.cs.unc.edu/~geom/RVO/icra2008.pdf">https: </a><a href="https://www.cs.unc.edu/~geom/RVO/icra2008.pdf">//www.cs.unc.edu/</a><a href="https://www.cs.unc.edu/~geom/RVO/icra2008.pdf"><sup>∼</sup></a><a href="https://www.cs.unc.edu/~geom/RVO/icra2008.pdf">geom/RVO/icra2008.pdf</a><a href="https://www.cs.unc.edu/~geom/RVO/icra2008.pdf">.</a></li>

</ol>