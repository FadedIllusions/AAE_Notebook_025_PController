# AAE_Notebook_025_PController
![Control Stack](/images/control_stack.png)

The only thing that we directly control are the multirotor's rotor rotation rates. These cause forces and moments on the multirotor; which, in turn, cause translational and rotational accelerations.

For now, let's focus only on the Controls vs Accelerations blocks of the control stack and ignore the rest. So, for our purposes, we have direct control over acceleration but not velocity nor position. If we want to change those quantities, we have to do so through acceleration.

![U1 Acceleration](/images/u1.png)

We use the variable 'u' to represent a thrust or torque control input to the multirotor. In the above picture, we're using u1 to represent the total thrust (in Newtons).

For a 1D monorotor, we have the equation of motion z_dot_dot = g - u1/m. Note that, with u1, we can directly influence acceleration, z_dot_dot; thus, we often rewrite the equation into the simpler form of z_dot_dot = u1_bar. After we have computed a desired acceleration, u1_bar, we can easily compute an u1 that would acheive that desired acceleration... u1_bar = g - u1/m.

*** Note: Velocity is the integral of Acceleration and Position is the integral of Velocity.

***   ***   ***   ***   ***   ***   ***   ***   ***

What happens if we overestimate our mass by some small amount and we have some non-zero acceleration epsilon?

(If we overestimate the mass of the vehicle, our control system would apply too much total thrust; thus, our vehicle would accelerate upward.)

![Mass Error](/images/mass_error.png)

What will happen to our position over time? 

z_dot_dot is our desired acceleration. Being as we've overestimated the mass of our vehicle and our controller, in turn, applies too much total thrust to the vehicle, creating an error between our desired position and our actual position, we can say that z_dot_dot is now our error, epsilon. (z_dot_dot = epsilon)

As seen in the above image, first, we integrate z_dot_dot to get velocity, z_dot; then, we integrate z_dot with respect to time to get position, z. With z(0) being z_0 and z_dot(0) being 0, we expect the altitude to remain at z_0. However, being as we have introduced an error into our controls, it grows quadratically with time, as seen in the third equation. All from a small error introduced to the mass (model error).

![Types of Errors](/images/error_types.png)

***   ***   ***   ***   ***   ***   ***   ***   ***

Thusfar, we've been working with an Open Loop Controller -- that is, a controller that does not receive any feedback.

![Open Loop Controller](/images/open_loop.png)

Now, let's start examining the implementation of Closed Loop Controllers, wherein the is some sort of measured feedback to help negate errors.

![Closed Loop Controller](/images/closed_loop.png)

Assuming a world with perfect sensors and having our reference, z_target; output, z; and input, u1... If we've defined error as e = z_target - z, when e > 0, how would the vehicle compensate? (Recall the z-axis to be positive downward.)

We'd expect our vehicle to descend, increasing z.

***   ***   ***   ***   ***   ***   ***   ***   ***

![Proportional Controller](/images/p_control.png)

Focusing on the Control block of the closed loop contoller, let's consider one of the most simple controllers, the P Controller (Proportional Control). (A Proportional Controller control strength is proportional to the error -- the larger the error, the more that the system has to do to correct that error.)
