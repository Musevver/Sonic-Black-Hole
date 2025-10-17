# Sonic Black Hole Simulator in Python

## Preface
**What this is:** 
- a passion project and learning opportunity.
- an educational tool that serves as a good intro to the dynamics of black hole analogs.

**What this isn't:**
- a fully accurate, research grade quantum simulation of a sonic black hole.

## Parameters
- `Nx` — number of spatial grid points (default 300). Improves spatial resolution, uses more memory.
- `Nt` — number of time steps (default 900). Gives longer runs and better frequency resolution.
- `CFL` — Courant factor controlling `dt` (typical 0.2–0.45). (timestep controller)
- `v_left`, `v_right` — asymptotic flow speeds of the acoustic on the left and right sides.
- `x_h` — nominal horizon position (where `v ≈ c`). (this is the simulated event horizon)
- `transition_width` (GUI label `trans_w`) — width of the tanh transition between `v_left` and `v_right`. Should be ≥ a few grid spacings. (this is how "steep" the event horizon is)
- `k0` — central wavenumber of the initial Gaussian wavepacket. (this is the frequency of the wavepacket)
- `packet_center`, `packet_width` — position and width of the initial packet (domain units). (where the packet is centered and how wide it is)
- `pml_fraction`, `sigma_max_factor` — PML absorber fraction and strength (reduce reflections at boundaries). (the simulation makes use of "sponges" on either side of the simulation to dampen the waves so as to minimize the effects of reflection)
- `Save outputs` (checkbox) — when enabled saves compressed outputs.

## Quick usage
Run to load the GUI, adjust parameters and click *Start Simulation*. Use *Animate* to replay stored snapshots. Note that you can only animate after you've simulated, if you animate after changing parameter without simulating it will animate the previous simulation rather than your new one.

## Technical background

For a barotropic, irrotational fluid (density $\rho$, sound speed $c$, background velocity $\mathbf v$ with $\mathbf v=\nabla\Psi$), linear perturbations of the velocity potential $\phi$ satisfy a wave equation that can be written as a covariant d'Alembertian in an *effective spacetime metric* (the sonic metric):

$$
\square_g \phi
= \frac{1}{\sqrt{-g}}\partial_\mu\big(\sqrt{-g}\,g^{\mu\nu}\partial_\nu\phi\big)=0
$$

with (up to an overall conformal factor)

$$
g_{\mu\nu} \propto
\begin{pmatrix}
-(c^2-\mathbf v^2) & -v_j \\
-v_i & \delta_{ij}
\end{pmatrix}
$$

In 1D this reduces to the convected wave equation the solver discretizes (second-order form):

$$
(\partial_t + v(x)\partial_x)^2\phi - c(x)^2\partial_x^2\phi = 0
$$

We evolve the equivalent first-order-in-time system in the code.



A sonic horizon occurs where the background flow equals the local wave speed:

$$
\text{horizon:}\quad v(x_H)=c(x_H)
$$

The analogue of black-hole surface gravity (1D) is

$$
\kappa \;=\; \tfrac{1}{2}\left.\frac{d}{dx}\big(c^2(x)-v^2(x)\big)\right|_{x=x_H}
$$

The code computes a finite-difference approximation to this derivative and reports the dimensionless Hawking temperature (Really profound that this analog is possible!)

$$
T_H^{(\mathrm{dimless})} \;=\; \frac{\kappa}{2\pi}
$$

which (with $\hbar$ and $k_B$ restored) gives $T_H=\hbar\kappa/(2\pi k_B)$


In stationary scattering one expands field solutions at frequency $\omega$ into incoming/outgoing modes (positive and negative norm). Scattering mixes them:

$$
\hat a^{\rm out}_\omega = \alpha_\omega \,\hat a^{\rm in}_\omega + \beta_\omega \,\hat a^{\rm in\,\dagger}_{-\omega}
$$

and the spontaneous emission (particle number per mode) is

$$
\langle n_\omega\rangle = |\beta_\omega|^2
$$

For near-horizon, stationary geometries a thermal (Planck) spectrum is predicted:

$$
|\beta_\omega|^2 \approx \frac{1}{\exp\!\big(\hbar\omega/(k_B T_H)\big)-1}
$$

In the code’s dimensionless units we compare observed $|\beta_\omega|^2$ to the dimensionless form $(e^{\omega/T_H^{(\mathrm{dimless})}}-1)^{-1}$







