!**********************************************************************
! Legal notice: UHYPER_Lefevre_Lopez-Pamies.for (Windows) 
!
! Copyright (C) 2018 Victor Lefèvre (victor.lefevre@northwestern.edu)
!                    Oscar Lopez-Pamies (pamies@illinois.edu)
!
! This ABAQUS UHYPER subroutine implements the hyperelastic energy  
! density derived in [1] for the macroscopic elastic response of
! isotropic and incompressible filled elastomers. The results applies
! to general non-percolative isotropic distributions of stiff 
! inclusions, stiff interphases, and stiff occluded rubber. This result
! is valid for any choice of I1-based incompressible energy density
! characterizing the non-Gaussian isotropic elastic response of the    
! underlying elastomer. The present subroutine is implemented for the  
! choice of strain energy density proposed in [2].
!
! For the special case of equiaxed particles surrounded by interphases  
! and occluded rubber, the model reduces to the result derived in [3]. 
! For the special case of equiaxed particles without interphases
! nor occluded rubber, both models reduce to the result obtained in
! [4]. 
!
! This program is free software: you can redistribute it and/or modify
! it under the terms of the GNU General Public License as published by
! the Free Software Foundation, either version 3 of the License, or
! (at your option) any later version.
!
! This program is distributed in the hope that it will be useful,
! but WITHOUT ANY WARRANTY; without even the implied warranty of
! MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
! GNU General Public License for more details.
!
! You should have received a copy of the GNU General Public License
! along with this program.  If not, see https://www.gnu.org/licenses/
!
!**********************************************************************
! Usage:
!
! The subroutine is to be used as an incompressible USER hyperelastic 
! model with either 8 (arbitrary microstructures) or 7 (equiaxed
! particles) material properties, e.g.,
! *HYPERELASTIC, USER, TYPE=INCOMPRESSIBLE, PROPERTIES=8
! or
! *HYPERELASTIC, USER, TYPE=INCOMPRESSIBLE, PROPERTIES=7
! in the input (.inp) file.
!
! 1/ For arbitrary microstructures, the 8 materials properties required
! by the model to be input to the subroutine via the PROPS array are
! listed in the table below:
!
!  AMU1    = PROPS(1)  ! PARAMETER #1 OF THE ELASTOMER
!  ALPHA1  = PROPS(2)  ! EXPONENT #1 OF THE ELASTOMER
!  AMU2    = PROPS(3)  ! PARAMETER #2 OF THE ELASTOMER
!  ALPHA2  = PROPS(4)  ! EXPONENT #2 OF THE ELASTOMER 
!  ACP     = PROPS(5)  ! VOLUME FRACTION OF PARTICLES 
!  ACI     = PROPS(6)  ! VOLUME FRACTION OF INTERPHASE 
!  ACO     = PROPS(7)  ! VOLUME FRACTION OF OCCLUDED RUBBER 
!  AMUT    = PROPS(8)  ! INITIAL SHEAR MODULUS OF FILLED ELASTOMER 
!
! The two material parameters AMU1, AMU2 characterizing the elastic    
! behavior of the underlying elastomer are non-negative real numbers 
! (AMU1 >= 0, AMU2 >= 0) with strictly positive sum (AMU1 + AMU2 > 0). 
! The two exponents ALPHA1, ALPHA2 are non-zero real numbers 
! (ALPHA1 ≠ 0, ALPHA2 ≠ 0) leading to a strongly elliptic strain 
! energy (see eq. (22) in [2]). This is left to the user to check.
!
! The volume fractions of particles (ACP), interphases (ACI), and
! and occluded rubber (ACO) must satisfy 0 <= ACP + ACI + ACO <= 1.
!
! The initial shear modulus of the filler elastomer AMUT must be a 
! non-negative real number (AMUT >= 0).
!
! 2/ For microstructures comprising stiff equiaxed particles,  
! interphases, and occluded rubber, the 7 materials properties required 
! by the model to be input to the subroutine via the PROPS array are
! listed in the table below:
!
!  AMU1    = PROPS(1)  ! PARAMETER #1 OF THE ELASTOMER
!  ALPHA1  = PROPS(2)  ! EXPONENT #1 OF THE ELASTOMER
!  AMU2    = PROPS(3)  ! PARAMETER #2 OF THE ELASTOMER
!  ALPHA2  = PROPS(4)  ! EXPONENT #2 OF THE ELASTOMER 
!  ACP     = PROPS(5)  ! VOLUME FRACTION OF PARTICLES 
!  ACI     = PROPS(6)  ! VOLUME FRACTION OF INTERPHASE 
!  ACO     = PROPS(7)  ! VOLUME FRACTION OF OCCLUDED RUBBER
!
! These 7 material properties are subjected to the same restrictions 
! listed above.
!
!**********************************************************************
! Additional information:
!
! This subroutine does not create solution-dependent state variables
! nor predefined field variables. 
!
! Please consult the ABAQUS Documentation for additional references 
! regarding the use of incompressible USER hyperelastic models with
! the UHYPER subroutine.
!
! Due the incompressible nature of this model, use of hybrid elements 
! is strongly recommended.
!
!**********************************************************************
! References:
!
! [1] Lefèvre, V., Lopez-Pamies, O. 2017. Nonlinear electroelastic 
!     deformations of dielectric elastomer composites: II — Non-Gaus-
!     sian elastic dielectrics. J. Mech. Phys. Solids  99, 438--470.
! [2] Lopez-Pamies, O., 2010. A new I1-based hyperelastic model for 
!     rubber elastic materials. C. R. Mec. 338, 3--11.
! [3] Goudarzi, T., Spring, D.W., Paulino, G.H., Lopez-Pamies, O., 2015
!     . Filled elastomers: a theory of filler reinforcement based on 
!     hydrodynamic and interphasial effects. J. Mech. Phys. Solids 
!     80, 37--67.
! [4] Lopez-Pamies, O., Goudarzi, T., Danas, K., 2013. The nonlinear 
!     elastic response of suspensions of rigid inclusions inr ubber: 
!     II—a simple explicit approximation for finite-concentration 
!     suspensions. J. Mech. Phys. Solids 61, 19--37.
!
!**********************************************************************