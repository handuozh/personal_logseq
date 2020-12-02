---
title: Point Feature Representation
---

## There are two main parameterizations of a 3D point feature: 3D position (xyz) and inverse depth with bearing.
## Both of these can either be represented in the global frame or in an anchor frame of reference which adds a dependency on having an "anchor" pose where the feature is observed.
## To allow for a unified treatment of different feature parameterizations $\boldsymbol \lambda\f$ in our codebase, 
we derive in detail the generic function ${}^{G}\mathbf{p}_f=\mathbf f (\cdot)\f$ that maps different representations into global position.
## Both of these can either be represented in the global frame or in an anchor frame of reference which adds a dependency on having an "anchor" pose where the feature is observed.
To allow for a unified treatment of different feature parameterizations \f$\boldsymbol \lambda\f$ in our codebase, 
we derive in detail the generic function \f${}^{G}\mathbf{p}_f=\mathbf f (\cdot)\f$ that maps different representations into global position.
