Download Link: https://assignmentchef.com/product/solved-mgt483-project-robust-regression
<br>
Linear regression relates a multivariate <em>input x </em>∈ R<em><sup>d </sup></em>to a scalar <em>output y </em>∈ R via a linear model, that is, <em>y </em>≈ <em>θ</em><sup>&gt;</sup><em>x </em>for some parameter vector <em>θ </em>∈ R<em><sup>d</sup></em>. The goal of linear regression is to learn <em>θ </em>from a finite dataset. In practical applications only part of the output can be explained by the inputs, and therefore we have <em>y</em><sup>(<em>i</em>) </sup>= <em>θ</em><sup>&gt;</sup><em>x</em><sup>(<em>i</em>) </sup>+ <em>ξ</em><sup>(<em>i</em>)</sup>, where the error <em>ξ</em><sup>(<em>i</em>) </sup>is a realization of a noise random variable with zero mean. In this case, the best one can hope for is to find an estimator <em>θ</em><sub>b </sub>≈ <em>θ </em>constructed from the data. The estimator <em>θ</em><sub>b </sub>can be used to predict the output <em>y </em>corresponding to a new unseen input <em>x</em>, which is at the core of many applications in statistics and machine learning. We assume throughout this project that = 1 for all <em>i </em>∈ [<em>N</em>], and thus <em>θ</em><sub>1 </sub>can be interpreted as a bias term that represents the mean or the median of <em>y</em><sup>(<em>i</em>)</sup>.

In this project you will derive and implement several linear programmingbased methods to learn the unknown parameter vector <em>θ </em>from data.

Figure 1: MSE vs. MAE estimators for a dataset with 1 outlier (red dot)

<h1>2           Mean-Absolute-Error Penalized Regression</h1>

There exist different methods to construct the estimator <em>θ</em><sub>b</sub>. The most widely used approach minimizes the mean-squared-error (MSE) corresponding to the given data to obtain the MSE estimator

<em>N</em>

<em>θ</em><sub>bMSE </sub>∈ argmin                                       <em>.       </em>(1)

<em>i</em>=1

Alternatively, one can minimize the mean-absolute-error (MAE) to obtain the more robust MAE estimator

<em>N</em>

<em>θ</em><sub>bMAE </sub>∈ argmin                                        <em>.       </em>(2)

<em>i</em>=1

<strong>Question 1 </strong>(5 points)<strong>. </strong><em>Figure </em><em>1 visualizes the outputs predicted by the MSE and MAE estimators on a dataset with 1 outlier. Explain the difference between the two estimators intuitively. Hint: Compare how </em>(1) <em>and </em>(2) <em>penalize errors.</em>

In case of high-dimensional inputs (<em>d &gt; N</em>), it makes sense to seek a <em>sparse </em>parameter vector <em>θ</em>. This means that many components of <em>θ </em>should be zero. The non-zero components of <em>θ </em>then correspond to the key features or key inputs that have a significant impact on the outputs. Sparsity can be induced by adding an <em>`</em><sub>1</sub>-penalty to the objective function of (1) or (2), which yields the least absolute shrinkage and selection operator (LASSO) method of regression analysis. In case of the MAE objective, the resulting LASSO estimator satisfies

<em>N</em>

<em>θ</em><sub>b</sub>∈ argmin                                   <em>,       </em>(3)

<em>i</em>=1

where denotes the <em>`</em><sub>1</sub>-norm of <em>θ</em>, and <em>λ </em>≥ 0 is a hyperparameter that represents the weight of the penalty. In practice, <em>λ </em>is typically chosen by cross validation. That is, we randomly partition D into a training dataset D<sub>train </sub>and a validation dataset D<sub>val</sub>. We then solve (3) using only D<sub>train </sub>for different values of <em>λ </em>and select the estimator that has minimal MAE on D<sub>val</sub>.

<strong>Question 2 </strong>(15 points: 5+5+5)<strong>.</strong>

<ol>

 <li><em>Rewrite </em>(3) <em>as a linear program (not necessarily in standard form).</em></li>

 <li><em>Implement this linear program in </em>MATLAB <em>or </em>Python <em>and solve it on the </em>Diabetes Dataset <em>for λ </em>= 0<em>.</em>5<em>. Use the code skeletons provided on Moodle. The diabetes dataset links a feature vector x comprising patient information (age, sex, body mass index) and blood measurements (blood pressure, T-Cells, lipoproteins, thyroid, lamotrigine and blood sugar) to an output y quantifying the progression of the diabetes disease. Compute the MAE of the resulting estimator on the separate test sets provided on Moodle. Compare the test performance against the training performance. Hint: Do not forget to append a constant bias term to the features.</em></li>

 <li><em>Use cross-validation to tune the hyperparameter λ. Randomly select 75% of the data to construct </em>D<sub>train </sub><em>and use the rest of the data to construct </em>D<sub>val</sub><em>. Use 50 logarithmically spaced values between </em>[10<sup>−5</sup><em>,</em>10<sup>−1</sup>] <em>as candidates for λ, select the one performing best on the validation set in terms of MAE. Compare again the test performance against the training performance.</em></li>

</ol>

<h1>3           Convex Hulls</h1>

Standard linear programs involve only real decision variables. However, many applications require binary decision variables that are restricted to {0<em>,</em>1}. Such variables emerge, for example, if you have to decide whether a project should be implemented or not or whether an item should be loaded on a truck or not etc. A linear optimization problem with binary decision variables is given below.

s<em>.</em>t<em>.     x</em><sub>1 </sub>+ <em>x</em><sub>2 </sub>≤ 1         (4) <em>x</em><sub>1</sub><em>,x</em><sub>2 </sub>∈ {0<em>,</em>1}

Any binary linear program (such as problem (4)) is equivalent to a standard linear program, which is obtained by replacing the original feasible set with its convex hull, that is, the smallest convex set containing all feasible points.

<strong>Definition 3.1 </strong>(Convex hull)<strong>. </strong><em>The convex hull of an arbitrary (possible nonconvex) set X </em>⊂ R<em><sup>n </sup>is defined as</em>

conv(<em> .</em>

<strong>Question 3 </strong>(5 points)<strong>. </strong><em>Denote by X the feasible set of problem </em>(4)<em>. Describe the convex hull of X and find its vertices. Plot both X and </em>conv(<em>X</em>)<em>.</em>

<strong>Question 4 </strong>(5 points)<strong>. </strong><em>Replace the feasible set of problem </em>(4) <em>with its convex hull and solve the resulting linear program using </em>MATLAB <em>or </em>Python<em>. Report the optimal decision variables. Explain why they must be binary. Hint: Characterize the BFS of the convex hull of the feasible set.</em>

<h1>4           Zero-Sum Games</h1>

In the standard cross validatioin scheme described in Section 2, training and validation sets were constructed randomly. In the remainder of the project we will explore an adversarial cross validation procedure, where the training set is chosen by a fictitious adversary who aims to maximize the prediction error of our estimator. This procedure will give rise to a MinMax problem of the form:

min max <em>f</em>(<em>x,z</em>)<em>.           </em>(5) <em>x</em>∈X <em>z</em>∈Z

MinMax problems are zero-sum games, where one player’s profit is the other player’s loss. They are more intricate than standard minimization problems. We will see, however, that problem (5) can sometimes be simplified to a standard minimization problem if Z is convex and <em>f</em>(<em>x,z</em>) is concave in <em>z </em>for all <em>x </em>∈ X.

As an example, suppose you own $10,000, which you want to hide in your apartment. There are <em>I </em>hiding spots with capacities of <em>C<sub>i </sub></em>dollars, <em>i </em>∈ [<em>I</em>]. In case of a burglary, you want to lose as little money as possible. Assume that the police need <em>T </em>minutes to reach your apartment, and therefore any thief must leave within <em>T </em>minutes to avoid arrest. Assume further that the amount of money found in hiding spot <em>i </em>equals <em>x<sub>i</sub></em>·<em>z<sub>i</sub></em>·<em>p<sub>i</sub></em>, where <em>x<sub>i </sub></em>stands for the amount of money hidden, <em>z<sub>i </sub></em>denotes the amount of time the thief spends searching spot <em>i </em>and <em>p<sub>i </sub></em>represents a constant reflecting the difficulty of searching spot <em>i</em>. If the thief spends an amount of time exceeding 1<em>/p<sub>i </sub></em>in location <em>i</em>, then all the money hidden in that spot will be found. It therefore makes sense to restrict <em>z<sub>i </sub></em>≤ 1<em>/p<sub>i</sub></em>.

We seek a plan for hiding the money in the best possible way. Specifically, we hope to loose the least amount of money in the worst case, that is, if a very efficient thief shows up. This problem can be modeled as a zero-sum game against the thief (indeed, our loss is the thief’s profit).

<strong>Question 5 </strong>(20 points: 3+3+3+8+3)<strong>. </strong><em>This question will guide you through formulating and solving the problem of finding the best hiding strategy.</em>

<ol>

 <li><em>Characterize your decision variables and your feasible set.</em></li>

 <li><em>Characterize the thief’s decision variables and feasible set.</em></li>

 <li><em>Use the solutions of the first two questions to formulate a MinMax problem with objective function</em></li>

</ol>

<em>I</em>

<em>f</em>(<em>x,z</em>) = <sup>X</sup><em>x<sub>i</sub>z<sub>i</sub>p<sub>i</sub>.</em>

<em>i</em>=1

<ol start="4">

 <li><em>Dualize the inner maximization problem to reformulate the MinMax problem as a standard minimization problem, which can be addressed with linear programming solvers. Hint: The decision variables of the outer minimization problem represent constants for the inner maximization problem.</em></li>

 <li><em>Implement the resulting linear program in </em>Python <em>or </em>MATLAB <em>and solve it for the provided input data p,C </em>∈ R<em><sup>I </sup>and T </em>∈ R<em>. Use the code skeletons available from Moodle. Report the worst amount of money you lose, the optimal plan for hiding the money and the thief’s optimal search strategy. Hint: Think about the meaning of the problem’s dual variables.</em></li>

</ol>

<h1>5           Adversarial Training</h1>

Standard cross validation partitions D randomly into a training set D<sub>train </sub>and a validation set D<sub>val </sub>of prescribed cardinalities. Hence, it could happen that the training set contains no outliers (lucky) or all outliers (unlucky). The resulting estimator <em>θ</em><sub>b </sub>thus depends on the choice of the training set. We now propose an alternative approach to construct <em>θ</em><sub>b </sub>in an adversarial (worst-case optimal) manner. Specifically, we will train the regression model on the worst possible training set D<sub>train </sub>we could have possibly constructed from D.

We now construct the best estimator for the worst-case training set containing <em>k </em>= b0<em>.</em>75·<em>N</em>c samples, where b<em>y</em>c denotes the largest integer smaller or equal to <em>y</em>. To that end, we formulate a MinMax problem, where the inner maximization problem optimizes over the binary variables <em>z<sub>i</sub></em>, <em>i </em>∈ [<em>N</em>], corresponding to the <em>N </em>samples. The binary variable <em>z<sub>i </sub></em>is set to 1 if sample (<em>y</em><sup>(<em>i</em>)</sup><em>,x</em><sup>(<em>i</em>)</sup>) is included in the training set and to 0 otherwise. The outer optimization problem over <em>θ </em>aims to minimize the MAE with LASSO penalty in view of the worst-case training set. In summary, we thus find the following MinMax problem.

s<em>.</em>t<em>.            </em><sup>X</sup><em>z<sub>i </sub></em>= <em>k                                                                       </em>(6)

<em>i</em>=1

<em>z<sub>i </sub></em>∈ {0<em>,</em>1}      ∀<em>i </em>∈ [<em>N</em>]

This MinMax problem can be interpreted as a zero-sum game between the statistician and an evil adversary who selects the worst possible training set.

<strong>Question 6 </strong>(15 points)<strong>. </strong><em>Derive the convex hull of the adversary’s feasible set</em>

<em>,                        </em>(7)

<em>where k is an integer. Prove that your formulation is indeed the convex hull. Hint: Two sets A and B are equivalent if A </em>⊆ <em>B and B </em>⊆ <em>A.</em>

<strong>Question 7 </strong>(25 points: 10+10+5)<strong>.</strong>

<ol>

 <li><em>Show that the MinMax problem </em>(6) <em>is equivalent to the following linear program. Hint: Use the insights from Sections </em><em>3 and </em><em>4.</em></li>

</ol>

<em>N                            d</em>

min               <em>kα </em>+ X<em>β</em><em>i </em>+ <em>λ</em>X<em>b</em><em>i</em>

<em>θ,α,β<sub>i</sub>,b<sub>i</sub></em>

<em>i</em>=1                       <em>j</em>=1

s<em>.</em>t<em>.        α </em>+ <em>β<sub>i </sub></em>≥ (<em>y</em><sup>(<em>i</em>) </sup>− <em>θ</em><sup>&gt;</sup><em>x</em><sup>(<em>i</em>)</sup>)<em>/k  </em>∀<em>i </em>∈ [<em>N</em>] (8) <em>α </em>+ <em>β<sub>i </sub></em>≥ −(<em>y</em><sup>(<em>i</em>) </sup>− <em>θ</em><sup>&gt;</sup><em>x</em><sup>(<em>i</em>)</sup>)<em>/k               </em>∀<em>i </em>∈ [<em>N</em>] <em>β<sub>i </sub></em>≥ 0       ∀<em>i </em>∈ [<em>N</em>]

− <em>b<sub>j </sub></em>≤ <em>θ<sub>j </sub></em>≤ <em>b<sub>j             </sub></em>∀<em>j </em>∈ [<em>d</em>]

<em>Give an interpretation for β. Explain how one can extract the validation set from a solution of this problem.</em>

<ol start="2">

 <li><em>Implement the linear program </em>(8) <em>in </em>MATLAB <em>or </em>Python <em>using the skeleton code we provide on Moodle and solve it for 50 logarithmically spaced values of λ in the interval </em>[10<sup>−5</sup><em>,</em>10<sup>−1</sup>]<em>. For each of these values compute the MAE of the corresponding estimator on the validation set, and select the best robust estimator. To assess the benefits of robust cross validation over standard cross validation, compare the MAE of the robust estimator against the MEA of the estimator found in Question </em><em>2.</em><em>3. In both cases the MEA should be computed on the test set.</em></li>

 <li><em>For the robust estimator found above, compare also the MEA on the test set against the MEA on the training set. Is there a significant difference to what you observed in Question </em><em>2.</em><em>3.? Interpret your observations?</em></li>

</ol>

<h1>6           No More Toy-Examples/Optimization Prize</h1>

So far, you have applied different regression techniques to the “Diabetes” dataset. In this open question, we ask you to apply one of the regression techniques seen in Sections 2 and 5 to a regression problem of your liking.

<strong>Question 8 </strong>(10 points)<strong>. </strong><em>Apply one of the regression techniques seen in Sections </em><em>2 and </em><em>5 to a dataset of your choice. Hint: Good sources of datasets include </em><a href="https://www.kaggle.com/datasets"><em>Kaggle</em></a><em> and the </em><a href="https://archive.ics.uci.edu/ml/index.php"><em>UCI archive</em></a><a href="https://archive.ics.uci.edu/ml/index.php"><em>.</em></a><em> In answering this question, put emphasis on interpreting your results and on justifying the chosen optimization scheme.</em>

<strong>Question 9 </strong>(Voluntary bonus question, 10 points)<strong>. </strong><em>If you like, prepare a short presentation of your answer of Question </em><em>8 (about 5 minutes) for the exercise session on May 20th. This will allow you to present your dataset and insights to your fellow students. We will give you bonus points for volunteering to do the presentation, but we will not grade its contents. Everyone present in the exercise session will be able to vote on the best presentation. The group attracting the most votes will receive an optimization prize.</em>