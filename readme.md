# New Breakthrough in Array Signal Processing: SAMV Method for Efficient DOA Estimation Without Grid and Coherent Sources [with MATLAB Code]

Array Signal Processing Breakthrough: Comprehensive Analysis of Iterative Sparse Asymptotic Minimum Variance (SAMV) Method
User-parameter freedom, gridless estimation, anti-coherent sources, and robustness to few or no samples are still outstanding. In array signal processing, direction of arrival (DOA) estimation is a classic yet challenging problem. Traditional high-resolution methods such as MUSIC and ESPRIT, while theoretically superior, often face limitations in practical applications due to factors like insufficient number of samples, signal coherence, array geometry constraints, and complex noise environments. In recent years, sparse signal representation methods have provided new insights into solving these issues, but they still struggle with parameter tuning difficulties, grid restrictions, and singularity of the covariance matrix.
This paper introduces the classic paper "Iterative Sparse Asymptotic Minimum Variance Based Approaches for Array Processing" published in IEEE Transactions on Signal Processing (2013). It proposes a series of user-parameter-free iterative sparse asymptotic minimum variance (SAMV) methods that can simultaneously estimate signal power and noise variance, achieving optimality under asymptotic meaning (equivalent to maximum likelihood estimation). Additionally, the authors introduced a gridless SAMV-SML method, breaking through the fixed direction grid's limitation on estimation accuracy through one-dimensional optimization, realizing truly high-resolution and efficient DOA estimation.
This paper will guide you through detailing the mathematical model of this method, the characteristics of three SAMV variants, behavior under high and low signal-to-noise ratios, and rich simulation validation results. Finally, it will summarize its core innovation points.
I. Introduction: Why sparse array processing has become a research hotspot?
The number of snaps is sufficient (usually required), to ensure a reliable estimation of the sample covariance matrix.
Signal sources are uncorrelated (coherent sources will cause a "rank deficiency" in the subspace);
The array geometry is known and calibrated accurately.
In practical scenarios, such as rapid time-varying channels, coherent multipath propagation, and single-snapshot imaging (such as radar/SAR), these conditions are often unmet, leading to a sharp decline in the performance of subspace methods, and even complete failure.
1.2 The rise of sparse signal representations: Sparse representation theory indicates that under over-complete dictionaries (such as dense directional grids), Actual signals often only contain a few non-zero components. Therefore, We can transform the DOA estimation problem into a sparse reconstruction problem:
The ( ) is an overcomplete directional vector matrix, and the ( ) is a sparse source signal vector. Sparse solutions can be obtained through methods such as minimizing the norm or iterative weighted least squares (e.g., FOCUSS, IAA).
1.3 Deficiencies of existing sparse methods IAA (Iterative Adaptive Approximation): No need for parameter tuning, but the constructed covariance matrix may be singular when there is noise-free and few non-zero powers.
IAA‑R: Regularization has been added, but still limited by a fixed directional grid.
Common Issue: All grid-based power estimation methods, whose resolution is limited by the refinement of the directional grids, which in turn introduces a significant computational burden.
1.4 Contributions to the above-mentioned pain points, the authors of this paper have proposed a series of SAMV methods:
1  
Starting from the asymptotic minimum variance (AMV) criterion, we derive an efficient iterative estimator equivalent to ML.
2  
By retaining the explicit noise variance term, the covariance matrix singularity problem is avoided, which is applicable to any number of samples (including).
3  
Three different forms of SAMV iterative formulas (SAMV-0, SAMV-1, SAMV-2) are proposed, which are suitable for different sparsity and signal-to-noise ratio requirements.
4  
The SAMV-SML method is further proposed, which breaks through grid limitations by decomposing the ML cost function and performing one-dimensional searches for each source direction. This achieves asymptotic effective estimation without meshes.
5
Approximate expressions under high/low signal-to-noise ratio conditions are derived, revealing the intrinsic connection between the SAMV method and the periodogram (PER), IAA.
II. Data Model and Problem Formulation 2.1 Array Signal Model Consider an array of fully-oriented sensors receiving signals from narrow-band far-field sources. The observation vector for the i-th fast-sampling is:
where:
is the steering matrix;
is the source signal vector, assumed to be uncorrelated in time:,;
is zero-mean white noise:;
signals and noise are independent.
Thus, the covariance matrix of the observed data is:
The sample covariance matrix is:
2.2 Sparse Processing In practice, we do not know the true number and location of the signal sources in advance but define a dense directional grid (usually) and treat each grid point as a potential "source". Thus, the actual source positions correspond to a few non-zero elements in the vector, with the rest being zero or close to zero. This is the typical form of the sparse reconstruction problem.
2.3 Vectorized Covariance Model To utilize second-order statistics, we vectorize:
where, and. At the same time, the asymptotic covariance matrix is:
So, the problem is transformed into estimating the parameter vector from the vectorized sample covariance.
III. Asymptotic Minimum Variance (AMV) Estimator 3.1 AMV Criterion Porat and Friedlander proposed the AMV criterion (also known as asymptotic best consistent estimate) that states: For estimation problems based on second-order statistics, any consistent estimator has a lower bound for its covariance matrix, which can be achieved by minimizing the following cost function:
3.2 Iterative Solving Formula For the derivatives of and, set them to zero, and perform matrix algebraic operations (using the properties of Kronecker product), obtain the following iterative updating formula (the i-th iteration):
Among them. Note that these formulas contain the current estimated values and, therefore, must be implemented iteratively.
Important theoretical results (Appendix B of the paper proves): The above AMV estimator is equivalent to a stochastic maximum likelihood (Stochastic ML) estimator under the Gaussian assumption, thus it has asymptotic optimality.
3.3 Negative Power Issues and Corrections When the mesh is overcompleted (...). The above direct formula may produce negative power estimates. This is obviously unreasonable. For this purpose, The author enforces non-negativeness in each iteration step:
Similarly, the maximum value is compared with 0. However, to obtain a more concise and natural non-negative iterative form, the paper further proposes three SAMV variants.
4. Three variants of the Sparse AMV method (SAMV) 4.1 The core idea is observed in the complete AMV iterative formula. The main source of negative values is the "..." item. If we assume that the approximation is near optimal at the solution (which is reasonable under high signal-to-noise ratio), Then these items can be removed. Purely, Ensure a non-negative update formula.
Based on this idea, the author proposed three different versions of SAMV methods:
Method
Power update formula
Feature
SAMV-0 has strong sparsity, requiring high SNR.
SAMV-1 is equivalent to weighted least squares, robust.
SAMV‑2 Balanced Sparsity and Weak Target Detection
The noise variance update for all three methods is unified as follows:
4.2 Another interpretation of SAMV-1: Weighted least squares paper proof (Appendix C), the power estimation of SAMV-1 is equivalent to minimizing the following weighted least squares cost function:
Among them, the covariance matrix after removing the influence of the first component is obtained. This makes SAMV-1 have a clear statistical interpretation.
4.3 Comparison with IAA: The power update of SAMV-1 is written in explicit summation form:
The power update for IAA is:
The two methods are very similar, differing only in the inclusion of noise terms (i.e., IAA does not include a noise term, while SAMV method explicitly models the variance of the noise, ensuring that the covariance matrix is always singular and thus numerical stability is better.
4.4 Initialization and Implementation: The initialization uses periodogram estimation:
After each iteration:
1.  
Calculate based on the current and;
2.  
Update all and;
3.  
It typically takes 10 to 20 iterations for convergence.
5. Break through grid limitations: SAMV-SML method
5.1 Problem background: All power estimation methods based on fixed directional grids (including IAA, SPICE, SAMV-0/1/2) have an inherent flaw: estimation accuracy is limited by the grid spacing. Even if the true source is between two grid points, the final estimate can only take adjacent grid points, leading to system biases. Although encryption grids can reduce biases, the computation increases linearly with the number of grids, making it difficult for real-time systems.
5.2 Decomposition of the random ML cost function: The classic random ML estimation needs to optimize all direction parameters simultaneously, which is a high-dimensional nonlinear optimization problem, often unfeasible. The paper cleverly decomposes the ML cost function into:
Where only the direction of the i-th source and its power and noise depend on it. Specifically, the form is:
Parameters and are defined by (i.e., the covariance matrix after removing the i-th source).
5.3 One-dimensional optimization process: Thus, for each direction of the source, we can minimize by one-dimensional search (such as Nelder-Mead simplex method) under the premise of fixed and (obtained by SAMV estimation), thereby obtaining precise direction estimates without grids.
The entire SAMV-SML algorithm flow is as follows:
1. Use SAMV-2 (or other methods) to obtain initial grid directions.
2. Iterative loop:
Use the current parameters to update the covariance matrix and;
Update the power and noise using SAMV formula;
Perform one-dimensional optimization updates their values.
3.
Until convergence.
Due to the need to solve a series of one-dimensional optimization problems, the computational load is significantly less than that of high-dimensional joint optimization, while completely escaping grid constraints. Simulations show that the mean square error (MSE) of SAMV-SML in the high SNR region is significantly lower than grid-dependent methods and approaches the random Cramer-Rao lower bound.
VI. Approximation Analysis for High and Low Signal-to-Noise Ratios 6.1 Low SNR Approximation (...) At this time, by substituting the SAMV update formula:
SAMV-1: (i.e., periodogram)
SAMV-0: Multiplying the periodogram by a scaling factor
SAMV-2: Also a scaled version of the periodogram
Noise estimation:
This indicates that at extremely low SNR, SAMV-1 degenerates into the classical periodogram, while SAMV-0/2 require higher initialization powers to work effectively.
6.2 High SNR Approximation (...) At this point (ignoring noise terms), so:
The power update of SAMV-1 becomes:
Where... This exactly represents the IAA update formula. Therefore, in high SNR, SAMV-1 is equivalent to IAA, but due to the unified handling of the noise term, SAMV has better numerical robustness across the entire SNR range.
SAMV-0 and SAMV-2 provide more sparse solutions, but weak targets may be suppressed.
This approximate analysis provides theoretical guidance for algorithm selection: use SAMV-1 or SAMV-2 when low SNR or weak targets need to be preserved; use SAMV-0 when high SNR and extreme sparsity are desired.
VII. Simulation Experiments and Performance Evaluation 7.1 DOA Estimation Scenario (ULA, 12 elements) Settings: Three sources located at 35.11°, 50.15°, 55.05°, powers of 5dB, 3dB, 4dB. The number of shots N=120, SNR=25dB. Test independently sourced and coherent source (source 1 and source 3 fully coherent).
Results for independently sourced (see Figure 1) show PER: severe ambiguity, unable to distinguish between 50.15° and 55.05°.
IAA: sidelobe reduction but still not able to completely separate.
SPICE+ and MUSIC: Successful discrimination, with good performance.
SAMV-0/1/2: All successfully distinguished, with SAMV-0 having the sharpest spectral peaks.
SAMV-SML: Estimated to be the most accurate, with power values closest to the true values.
Coherent source results (see Figure 2) MUSIC and SPICE+: Performance severely degraded, leading to false peaks or missed detections.
IAA and SAMV-1: Still operable, but resolution slightly reduced.
SAMV-2: Clearly distinguishes all three sources, with excellent robustness.
SAMV-SML: Impact of coherence minimal, with the lowest MSE.
7.2 Angle estimation MSE variation with SNR (two sources case) separately evaluates independent sources and coherent sources, using fast-sampling N=16 and N=120.
PER MSE is highest, consistently larger.
SPICE+ performs well in independent sources and high-fast-sampling, but MSE sharply increases under coherent sources.
IAA/SAMV-1/SAMV-2 have similar MSE in low SNR region, verifying the approximate theory.
Grid-dependent algorithm exhibits platform effect at high SNR region: due to grid spacing of 0.2°, even with higher SNR, estimation error will not drop below about 0.06° (root mean square), which is system bias.
SAMV-SML completely eliminates platform effect, with MSE continuously decreasing with SNR increase, eventually close to CRB.
7.3 Single fast-sampling SISO radar imaging (Range-Doppler) simulation for 9 moving targets (3 weak targets of 5dB, 6 strong targets of 25dB), with received noise of 0dB.
Matched filtering (MF): Severe leakage, weak targets invisible.
IAA/SAMV-1: Significant improvement, all targets visible, but still a few sidelobes.
SAMV-0: Extremely sparse, sidelobes completely disappear, but weak targets are missed.
SAMV-2: A balance between sparsity and weak target preservation is achieved, with the image being the clearest.
When the weak target power is increased to 15dB, SAMV-0 can also achieve perfect imaging, further confirming its sensitivity to SNR.

Conclusion and core innovation summary:
8.1 Conclusion This paper systematically introduces a family of sparsely-approximated minimum variance (SAMV) methods for source localization and power estimation in array processing. Compared with existing techniques, the SAMV method has the following outstanding advantages:
No need for any user parameter tuning; fully adaptive;
Applicable to any number of fast shots (including single fast shot), without requiring the sample covariance matrix to be invertible;
Naturally handles coherent sources without the need for de-correlation preprocessing;
Supports arbitrary array geometries;
The SAMV-SML extension achieves ungridded approximate effective estimation, breaking through the resolution limit of fixed grids;
Simulation experiments show that the SAMV series methods outperform or rival mainstream algorithms (IAA, SPICE+, MUSIC, PER) under various challenging scenarios, especially SAMV-2 and SAMV-SML perform exceptionally well in weak target detection and accuracy.
8.2 Three major innovation points:
1. The first application of AMV criterion system to sparse array processing, proving its equivalent to random ML and thus achieving approximate optimal performance.
2.
Proposed three different sparseness characteristics of SAMV iterative formats, and their respective applicable signal-to-noise ratio conditions were given through theoretical analysis and numerical simulations.
3.
Innovated an ungridded SAMV-SML method based on the ML cost function decomposition, solving the grid limitation problem only through one-dimensional optimization, achieving near CRB estimation accuracy while maintaining low computational complexity.
8.3 Code and applicability The authors provide complete MATLAB implementation codes (available at https://qilin-zhang.github.io/publications/). This method has been successfully applied in multiple fields such as sonar, radar, wireless communication, and is particularly suitable for scenarios where traditional methods struggle to handle rapid changes, coherent multipath, and single pulse radar imaging.
References (Selected))[1] Abeida H., Zhang Q., Li J., Merabtine N. “Iterative sparse asymptotic minimum variance based approaches for array processing.” IEEE Trans. Signal Process., vol. 61, no. 4, pp. 933–944, 2013.[2] Yardibi T., Li J., Stoica P., et al. “Source localization and sensing: A nonparametric iterative adaptive approach based on weighted least squares.” IEEE Trans. Aerosp. Electron. Syst., 2010.[3] Stoica P., Babu P., Li J. “SPICE: A sparse covariance-based estimation method for array processing.” IEEE Trans. Signal Process., 2011.  
Conclusion: The SAMV method and its unstructured extension provide a powerful set of tools for array processing, inheriting the advantages of sparse reconstruction while achieving breakthroughs in numerical stability, user-friendliness, and estimation accuracy. If you are involved in DOA estimation, radar imaging, or source positioning, the SAMV is worth your deep exploration.
Welcome to leave comments in the comment section to discuss. If you have questions about the MATLAB code implementation or specific applications, feel free to message me for further discussion.

## Images

![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](01.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](02.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](03.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](04.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](05.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](06.jpg)
![New breakthrough in array signal processing: SAMV method achieves efficient DOA estimation without grid and anti-coherent sources [with MATLAB code]](07.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
