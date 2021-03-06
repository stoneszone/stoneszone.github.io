---
layout: page
toc: true
---
## Output-based adaptation for the CPR method
### Dual-consistent discretization of the CPR method
{% assign image_size='middle' %}
{% assign image_folder = 'adjoint' %}
{% assign number_of_images = 10 %}
{% include amc/make_gallery %}

## Adjoint-based h-adaptation for the CPR method
### Isotropic h-adaptation for the subsonic flow over the NACA0012 airfoil
{% assign image_size='middle' %}
{% assign image_folder = 'h-adapt-naca-inv-sub' %}
{% assign number_of_images = 10 %}
{% include amc/make_gallery %}

### Isotropic h-adaptation for the laminar flow over the NACA0012 airfoil
{% assign image_size='middle' %}
{% assign image_folder = 'h-adapt-naca-vis-sub' %}
{% assign number_of_images = 9 %}
{% include amc/make_gallery %}

### Anisotropic h-adaptation for the laminar flow over the NACA0012 airfoil
{% assign image_size='middle' %}
{% assign image_folder = 'h-adapt-naca-vis-sub-aniso' %}
{% assign number_of_images = 4 %}
{% include amc/make_gallery %}

### hp-adaptation for the CPR method
{% assign image_size='middle' %}
{% assign image_folder = 'hp-adapt' %}
{% assign number_of_images = 6 %}
{% include amc/make_gallery %}


## High-order hybrid PNPM-CPR framework
### PNPM-CPR method
{% assign image_size='middle' %}
{% assign image_folder = 'pnpm-cpr' %}
{% assign number_of_images = 3 %}
{% include amc/make_gallery %}

### Laminar flow over a cylinder using the PNPM-CPR method
<img style="display: block; margin: 0 auto;" width="400" height="240" align="middle" src="/img/cylinder/cylinder-p2p3.gif">
{% assign image_size='middle' %}
{% assign image_folder = 'cylinder' %}
{% assign number_of_images = 5 %}
{% include amc/make_gallery %}


### Viscous flow over the NACA-0012 airfoil using the PNPM-CPR method
{% assign image_size='middle' %}
{% assign image_folder = 'pnpm-naca12' %}
{% assign number_of_images = 5 %}
{% include amc/make_gallery %}


## Internal Aerodynamics
{% assign image_size='middle' %}
{% assign image_folder = 'internal-aerodynamics' %}
{% assign number_of_images = 9 %}
{% include amc/make_gallery %}

## Aircraft Inlet Design and Integration
{% assign image_size='middle' %}
{% assign image_folder = 'inlet-design' %}
{% assign number_of_images = 5 %}
{% include amc/make_gallery %}

## Computational electromagnetics
{% assign image_size='small' %}
{% assign image_folder = 'cem' %}
{% assign number_of_images = 4 %}
{% include amc/make_gallery %}

## Lattice Boltzmann method
### Neutrally buoyant particle in a linear shear flow
* The motion of a neutrally buoyant circular particle in the couette flow is simulated
* Multi-Relaxation-Time(MRT) Lattice Boltzmann Method(LBM) is used for hydrodynamic simulation
* Fluid/structure interactions are calculated by Immersed Boundary Method(IBM)
{% assign image_size='small' %}
{% assign image_folder = 'particle-couette' %}
{% assign number_of_images = 3 %}
{% include amc/make_gallery %}

<iframe style="padding:0.5em 0.3em;margin-left:auto;margin-right:auto;display:block;align:center;clear:both;" title="YouTube video player" width="480" height="120" src="http://www.youtube.com/embed/RZuPvglJY6E" frameborder="0">  </iframe>

<iframe style="padding:0.5em 0.3em;margin-left:auto;margin-right:auto;display:block;align:center;clear:both;" title="YouTube video player" width="480" height="120" src="http://www.youtube.com/embed/zammo9QyMg4" frameborder="0">  </iframe>

### Taylor-Couette flow using immersed boundary method (IBM) and interpolated bounce back (IBB)
* 2D Taylor-Couette flow using IBM and IBB
* Accuracy comparison between IBM and IBB for wall boundary condition
{% assign image_size='small' %}
{% assign image_folder = 'taylor-couette' %}
{% assign number_of_images = 2 %}
{% include amc/make_gallery %}
        
### Cylinder in a transient Couette flow using IBB
* Galerkin Invariant is verified by using fixed and moving coordinate
{% assign image_size='middle' %}
{% assign image_folder = 'cylinder-couette' %}
{% assign number_of_images = 2 %}
{% include amc/make_gallery %}

### Electroosmosis simulation using LBM/IBM
{% assign image_size='middle' %}
{% assign image_folder = 'electroosmosis' %}
{% assign number_of_images = 2 %}
{% include amc/make_gallery %}


