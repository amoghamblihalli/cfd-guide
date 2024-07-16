# Steps
1. Select CFD Model depending on application.
2. Calculate y+ -> yp -> yh.
3. Calculate growth rate.

# Model

| **Model**             | **Full Name**                          | **Description**                                                                                 | **Typical Applications**                                                                   | **Advantages**                                                                                              | **Limitations**                                                                           | **Recommended y+** | **Time Complexity** | **Empirical Nature** | **Reynolds Number Range** | **Special Notes / Cautions**                                                           | **Requires Boundary Conditions**          | **Wall Conditions**       | **Mesh (Type, Size)**                        | **Verifying Simulation Accuracy**                                               |
| --------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------ | ------------------- | -------------------- | ------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------- | ------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------- |
| **Laminar**           | Laminar Flow Model                     | Solves Navier-Stokes equations without turbulence modeling.                                     | Low Reynolds number flows, microscale flows.                                               | Simple, less computationally intensive, suitable for low Reynolds number flows.                             | Inaccurate for turbulent flows, cannot handle high Reynolds number.                       | N/A                | Low                 | None                 | \( Re < 2300 \)           | Ensure Reynolds number is low to avoid transition to turbulence.                       | Inlet/Outlet, No-Slip Walls               | No-Slip                   | Structured/Unstructured, Fine                | Validate against analytical solutions or low Reynolds number experimental data. |
| **Standard k-ε**      | Standard k-Epsilon Model               | Two-equation model solving for turbulence kinetic energy (k) and dissipation rate (ε).          | General-purpose, industrial flows, HVAC.                                                   | Robust, widely used, good for high Reynolds numbers, well-validated.                                        | Less accurate for complex flows and near-wall regions.                                    | y+ ~ 30-300        | Medium              | High                 | \( Re > 5000 \)           | Avoid for low Reynolds number or flows with strong curvature and separation.           | Inlet/Outlet, Wall Functions              | Wall Functions            | Structured/Unstructured, Coarse              | Compare with empirical data and standard turbulence benchmarks.                 |
| **RNG k-ε**           | Re-Normalization Group k-Epsilon Model | Variation of standard k-ε with additional terms for rotation and strain effects.                | Flows with high strain rates, swirling flows.                                              | Improved accuracy over standard k-ε, handles high strain and rotation better.                               | More computationally intensive than standard k-ε.                                         | y+ ~ 30-300        | Medium              | High                 | \( Re > 5000 \)           | More accurate for swirling and highly strained flows; more sensitive to grid quality.  | Inlet/Outlet, Wall Functions              | Wall Functions            | Structured/Unstructured, Coarse to Medium    | Validate with experimental data for high strain and rotating flows.             |
| **Realizable k-ε**    | Realizable k-Epsilon Model             | Another k-ε variation with different formulation for ε equation.                                | Flows with complex secondary motions, free-shear flows.                                    | Better performance for flows with strong streamline curvature, more accurate in predicting spreading rates. | More complex than standard k-ε.                                                           | y+ ~ 30-300        | Medium              | High                 | \( Re > 5000 \)           | Ensure proper validation for specific applications as model formulations vary.         | Inlet/Outlet, Wall Functions              | Wall Functions            | Structured/Unstructured, Coarse to Medium    | Compare with literature data for complex secondary flows.                       |
| **Standard k-ω**      | Standard k-Omega Model                 | Two-equation model solving for turbulence kinetic energy (k) and specific dissipation rate (ω). | Aerodynamic flows, external flows.                                                         | Accurate near-wall treatment, good for boundary layers, less sensitive to free-stream turbulence.           | Sensitivity to free-stream turbulence, requires careful selection of boundary conditions. | y+ ~ 1             | Medium              | Medium               | \( Re > 1000 \)           | Sensitive to free-stream boundary conditions; adjust accordingly.                      | Inlet/Outlet, Free-stream, Wall Functions | Wall Functions            | Structured/Unstructured, Fine                | Validate against aerodynamic data and boundary layer profiles.                  |
| **SST k-ω**           | Shear Stress Transport k-Omega Model   | Combines k-ω near walls and k-ε in the free-stream.                                             | Flows with adverse pressure gradients, separation, external aerodynamics.                  | Improved accuracy in adverse pressure gradients and separation, good near-wall treatment.                   | More complex and computationally intensive than standard models.                          | y+ ~ 1             | Medium to High      | Medium               | \( Re > 1000 \)           | Good compromise for various flow types; check transition to turbulence criteria.       | Inlet/Outlet, Free-stream, Wall Functions | Wall Functions            | Structured/Unstructured, Fine to Medium      | Compare with separation and adverse pressure gradient data.                     |
| **RSM**               | Reynolds Stress Model                  | Solves transport equations for Reynolds stresses.                                               | Complex turbulent flows, rotating flows, anisotropic turbulence.                           | Handles complex turbulence, anisotropic effects, better accuracy for swirling flows.                        | Very computationally intensive, difficult to converge, complex to set up.                 | y+ ~ 1-5           | High                | Low                  | \( Re > 1000 \)           | Useful for highly anisotropic turbulence; ensure adequate computational resources.     | Inlet/Outlet, Wall Functions              | Wall Functions            | Structured/Unstructured, Fine                | Validate against detailed experimental data for complex flows.                  |
| **LES**               | Large Eddy Simulation                  | Resolves large turbulent structures, models smaller ones.                                       | High Reynolds number flows, transitional flows, combustion.                                | High accuracy, captures transient effects, better prediction of complex flows.                              | Very computationally demanding, requires fine mesh and time-stepping.                     | y+ ~ 1             | Very High           | Low                  | \( Re > 10^4 \)           | Requires significant computational resources and fine spatial and temporal resolution. | Inlet/Outlet, Wall Functions              | Wall Functions or No-Slip | Structured/Unstructured, Very Fine           | Compare with time-resolved experimental data and benchmark LES studies.         |
| **DES**               | Detached Eddy Simulation               | Hybrid model combining RANS and LES.                                                            | Aerodynamics, external flows with large separation, automotive and aerospace applications. | Combines benefits of RANS and LES, balances accuracy and computational cost.                                | Complex, requires careful grid design and model setup.                                    | y+ ~ 1-5           | High                | Medium               | \( Re > 10^4 \)           | Ensure appropriate grid resolution and transition from RANS to LES regions.            | Inlet/Outlet, Wall Functions              | Wall Functions or No-Slip | Structured/Unstructured, Fine to Medium      | Validate with large separation and high Reynolds number data.                   |
| **DNS**               | Direct Numerical Simulation            | Resolves all scales of turbulence without modeling.                                             | Fundamental turbulence research, benchmark studies.                                        | Most accurate, no turbulence modeling assumptions.                                                          | Extremely computationally demanding, impractical for most engineering applications.       | y+ ~ 1             | Extremely High      | None                 | \( Re < 10^4 \)           | Used primarily for research and validation due to high computational costs.            | Inlet/Outlet, No-Slip Walls               | No-Slip                   | Structured/Unstructured, Very Fine           | Compare with theoretical solutions and detailed experimental data.              |
| **VOF**               | Volume of Fluid                        | Tracks the interface between two immiscible fluids.                                             | Multiphase flows, free surface flows, sloshing, mixing.                                    | Accurate interface tracking, handles large density differences.                                             | Requires fine mesh near interface, can be computationally expensive.                      | N/A                | Medium to High      | Low                  | Any                       | Use fine mesh near interface for better accuracy; ensure proper phase initialization.  | Inlet/Outlet, Phase Interface             | No-Slip or Moving Wall    | Structured/Unstructured, Fine near interface | Validate with experimental data on phase interfaces and multiphase benchmarks.  |
| **Mixture Model**     | Mixture Model                          | Solves for volume fractions of different phases using a single set of momentum equations.       | Bubbly flows, slurry flows, homogeneous multiphase flows.                                  | Simpler than VOF, good for homogeneous mixtures, less computationally intensive.                            | Less accurate for sharp interfaces, not suitable for large density differences.           | N/A                | Medium              | Medium               | Any                       | Suitable for well-mixed phases; less accurate for interfaces.                          | Inlet/Outlet, Phase Interface             | No-Slip                   | Structured/Unstructured, Medium              | Compare with well-mixed multiphase flow data.                                   |
| **Eulerian Model**    | Eulerian Multiphase Model              | Separate momentum equations for each phase.                                                     | Complex multiphase flows, sediment transport, fluidized beds.                              | Handles complex interactions between phases, can model granular flows.                                      | Very computationally intensive, complex setup and convergence issues.                     | N/A                | High                | Medium               | Any                       | Requires detailed phase interaction data; complex to set up and converge.              | Inlet/Outlet, Phase Interface             | No-Slip or Moving Wall    | Structured/Unstructured, Medium to Fine      | Validate with detailed multiphase experimental data.                            |
| **Species Transport** | Species Transport Model                | Solves for transport of multiple chemical species with or without reactions.                    | Combustion, pollutant dispersion, mixing, chemical reactors.                               | Handles complex chemical reactions, detailed modeling of species transport.                                 | Requires detailed kinetic data, can be computationally intensive, complex setup.          | N/A                | Medium to High      | Medium               | Any                       | Ensure accurate reaction kinetics and boundary conditions; computationally intensive.  | Inlet/Outlet, Species Boundary            | No-Slip or Catalytic Wall | Structured/Unstructured, Medium to Fine      | Compare with experimental data on species concentrations and reaction rates.    |
| **DO**                | Discrete Ordinates Radiation Model     | Solves radiative transfer equation in multiple discrete directions.                             | Radiative heat transfer, combustion, furnaces, participating media.                        | Handles complex geometries and participating media, can model absorption, emission, and scattering.         | Can be computationally intensive, requires fine angular discretization.                   | N/A                | High                | Medium               | Any                       | Fine angular discretization needed for accuracy; computational cost can be high.       |                                           |                           |                                              |                                                                                 |

| **P-1** | P-1 Radiation Model                | Simplified radiation model using diffusion approximation.           | Combustion, furnace applications, simplified radiative heat transfer. | Simple, less computationally intensive, suitable for optically thick media.                         | Less accurate for optically thin media and complex geometries, diffusion approximation. | N/A | Low  | High   | Any | Suitable for optically thick media; limited accuracy in optically thin conditions. |
| ------- | ---------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --- | ---- | ------ | --- | ---------------------------------------------------------------------------------- |

# Mesh

| **Mesh Type**                      | **Dimensions** | **Shape**                                           | **Description**                                              | **Advantages**                                                                      | **Disadvantages**                                                                    | **Typical Applications**                                            | **When to Use**                                                                  | **How to Use**                                                                         | **Suitable For**                                                         | **Special Notes**                                                          | **Computation Time** | **Sims**                 |
| ---------------------------------- | -------------- | --------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------- | -------------------- | ------------------------ |
| **1D Mesh**                        | 1D             | Line Elements                                       | Mesh composed of linear elements.                            | Simplest to set up, very fast computations.                                         | Limited to problems with one spatial dimension, not suitable for complex geometries. | Beam analysis, trusses, cables.                                     | Problems with one predominant dimension.                                         | Define nodes along the single dimension of interest.                                   | Structural analysis of beams and trusses.                                | Not suitable for 2D or 3D flow problems.                                   | Very Low             | Structural analysis      |
| **2D Mesh**                        | 2D             | Triangles                                           | Mesh composed of triangular elements.                        | Good for capturing complex geometries.                                              | Can create higher computational cost compared to quadrilaterals.                     | Flow over airfoils, thin plate heat transfer.                       | For irregular and complex geometries.                                            | Define nodes in two dimensions, refine around areas of interest.                       | CFD simulations of 2D flows, thermal analysis.                           | Suitable for complex shapes.                                               | Low to Medium        | CFD, structural, thermal |
| **2D Mesh**                        | 2D             | Quadrilaterals                                      | Mesh composed of quadrilateral elements.                     | More efficient than triangles for structured regions.                               | Poor adaptability to complex geometries.                                             | Flow in ducts, simple aerodynamic shapes.                           | For simple, regular geometries.                                                  | Define nodes in two dimensions, refine around areas of interest.                       | CFD simulations of 2D flows, thermal analysis.                           | Efficient for regular shapes.                                              | Low to Medium        | CFD, structural, thermal |
| **3D Mesh**                        | 3D             | Tetrahedrons                                        | Mesh composed of tetrahedral elements.                       | Suitable for capturing complex 3D geometries.                                       | Higher computational cost.                                                           | Flow around complex objects.                                        | For highly irregular and complex geometries.                                     | Define nodes in three dimensions, use finer mesh in regions of high gradients.         | Detailed CFD simulations, structural analysis.                           | Provides good accuracy for complex geometries.                             | High                 | CFD, structural, thermal |
| **3D Mesh**                        | 3D             | Hexahedrons                                         | Mesh composed of hexahedral elements.                        | Better numerical accuracy and efficiency.                                           | Complex to generate for irregular geometries.                                        | Internal flow in pipes, simple aerodynamic shapes.                  | For simple or moderately complex geometries.                                     | Define nodes in three dimensions, use finer mesh in regions of high gradients.         | CFD simulations, thermal and structural analyses.                        | Difficult to use for complex shapes.                                       | Medium to High       | CFD, structural, thermal |
| **3D Mesh**                        | 3D             | Pyramids                                            | Mesh composed of pyramid elements.                           | Useful for transition regions between hex and tet meshes.                           | Complex to generate, can lead to grid alignment issues.                              | Flow in transition regions of complex domains.                      | For areas needing smooth transition between different element types.             | Define nodes to smoothly transition between hex and tet elements.                      | CFD simulations in mixed complexity domains.                             | Ensures smooth transition between mesh types.                              | Medium               | CFD, structural, thermal |
| **3D Mesh**                        | 3D             | Prisms                                              | Mesh composed of prismatic elements.                         | Good for boundary layers in CFD.                                                    | Complex to generate, higher computational cost.                                      | Boundary layer flows.                                               | For capturing boundary layer effects accurately.                                 | Define nodes in three dimensions, use prismatic elements near boundaries.              | CFD simulations of boundary layers.                                      | Provides high accuracy near walls.                                         | Medium               | CFD                      |
| **Structured Mesh**                | 2D, 3D         | Quadrilaterals, hexahedrons                         | Mesh with regular grid pattern.                              | Easier to generate, efficient for simple geometries.                                | Poor adaptability to complex geometries, grid alignment issues.                      | Flow in pipes, heat exchangers, simple aerodynamic shapes.          | For simple and regular geometries.                                               | Define a grid with regular spacing, align with flow direction if possible.             | CFD simulations, thermal and structural analyses of regular shapes.      | Can be inefficient for highly complex geometries.                          | Low to Medium        | CFD, structural, thermal |
| **Unstructured Mesh**              | 2D, 3D         | Triangles, tetrahedrons, mixed elements             | Mesh with irregular grid pattern.                            | Adaptable to complex geometries, allows local refinement.                           | More difficult to generate, higher computational cost.                               | Flow around complex objects, automotive and aerospace applications. | For highly complex or irregular geometries.                                      | Define nodes to capture complex boundaries, refine mesh where necessary.               | CFD simulations, detailed structural analyses.                           | Provides better accuracy for complex geometries.                           | Medium to High       | CFD, structural, thermal |
| **Hybrid Mesh**                    | 2D, 3D         | Combination of structured and unstructured elements | Combines structured and unstructured elements.               | Combines benefits of both mesh types, adaptable and efficient.                      | Complex to set up and manage, potential for grid incompatibilities.                  | Flow in complex domains with simpler subregions.                    | When different regions of the domain require different mesh types.               | Combine structured mesh in simple regions with unstructured mesh in complex regions.   | CFD simulations in mixed complexity domains, multiphysics problems.      | Careful transition management between mesh types required.                 | Medium to High       | CFD, structural, thermal |
| **Adaptive Mesh Refinement (AMR)** | 2D, 3D         | Dynamically refined elements                        | Mesh with automatic refinement in regions of high gradients. | Efficient use of computational resources, accurate.                                 | Complex to implement, requires dynamic mesh adaptation algorithms.                   | Transient flow simulations, shockwave analysis, combustion.         | For problems with localized high gradients or moving boundaries.                 | Set criteria for refinement based on solution gradients, implement dynamic adaptation. | Detailed CFD simulations of transient and complex phenomena.             | Can significantly reduce computational cost while maintaining accuracy.    | Medium to High       | CFD                      |
| **Cartesian Mesh**                 | 2D, 3D         | Rectangular, cubical elements                       | Mesh with regular rectangular grid.                          | Simplifies computational algorithms, efficient for simple geometries.               | Poor adaptability to curved boundaries, stair-stepping effects.                      | Internal flow in ducts, heat transfer in rectangular domains.       | For geometrically simple domains aligned with Cartesian coordinates.             | Define a grid with regular Cartesian spacing, use refinement as needed.                | CFD simulations of simple internal flows, thermal analysis.              | Not suitable for highly complex or curved geometries.                      | Low                  | CFD, structural, thermal |
| **Body-Fitted Mesh**               | 2D, 3D         | Conforms to complex geometries                      | Mesh that accurately conforms to complex boundaries.         | Accurate representation of complex boundaries, better accuracy for curved surfaces. | More complex to generate, higher computational cost.                                 | Flow around aerodynamic bodies, biological flow simulations.        | When accurate boundary representation is crucial.                                | Generate mesh that conforms to body surfaces, refine near boundaries.                  | Detailed CFD simulations of external flows, complex structural analysis. | Requires detailed geometry and careful meshing.                            | Medium to High       | CFD, structural          |
| **Polyhedral Mesh**                | 3D             | Polyhedrons                                         | Mesh composed of polyhedral elements.                        | Good accuracy with fewer elements, efficient for complex geometries.                | More complex mesh generation, higher computational cost for element definition.      | Complex internal and external flows, multiphase flow.               | For complex geometries where polyhedral elements can reduce the number of cells. | Generate polyhedral cells from an initial tetrahedral mesh, refine as needed.          | CFD simulations of complex geometries, multiphase flows.                 | Can improve accuracy and reduce cell count compared to tetrahedral meshes. | Medium to High       | CFD                      |
