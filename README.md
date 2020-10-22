# selective_reduced_integration_SRI-dealii

Implementation of helper functions to use selective reduced integration for the assembly of the RHS.
For instance, you provide a stress and tangent, and we extract the volumetric-deviatoric or normal-shear parts from it to integrate the deviatoric or normal part by full integration and the volumetric or shear part via a reduced integration scheme.

@todo Check effect of normal-shear split for shear locking for instance bending

@todo Currently vol-dev split gives a very small convergence radius. Is this due to local minimum from locking-dominated solution, is this some other issue or common for other locking-free element?

## DOCU missing!!

## Modifications/Extensions to existing code

0. Assembly routine & compute global force

1. Declarations

```
	const QGauss<dim>                qf_cell;
	const QGauss<dim>                qf_center;

	const QGauss<dim - 1>            qf_face;
	const unsigned int               n_q_points;
	const unsigned int				 n_SRI_qp;
	const unsigned int               n_q_points_f;
```

2. Constructor

```
qf_cell( degree +1 + ( (parameter.reducedIntegration) ? -1 : 0 ) ),	// switch to reduced integration
qf_center( (parameter.use_SRI) ? (degree +1 -1) : 0 ),
qf_face( degree +1 + ( (parameter.reducedIntegration) ? -1 : 0 ) ),
n_q_points (qf_cell.size() ),
n_SRI_qp ( qf_center.size() ),
n_q_points_f (qf_face.size()),
```

3. QP history

Quadrature point history needs to be prepared for total number of quadrature points (n_q_points+n_SRI_qp)


4. History update + general

update_history for each quadrature point (in general check every occurence of n_q_points