# kalmancpp simple kalman filter c++ header only library ## Fetures

- Kalman filter
- Extended kalman filter

# Dependencies

- lincpp(header only)

# Usage

```c++

lin::Mat<double> F(lin::Mat<double> x, lin::Mat<double> u){ // state transition function
    // n  is the size of the state matrix
    lin::Mat<double> out(n,1);
    /*
    Your code, that tranforms state vector x and control vector to next
    state vector
    simpliest example:
    out = x; // state = previous state.
    this function is used to predict the next state and to calculate the jacobian if an analitic function is not provided
    */
    return out;
}

lin::Mat<double> G(lin::Mat<double> x){ // observation transition function
    // m is the size of the observation matrix
    lin::Mat<double> out(m,1);
    /*
    Your code, that transforms the state vector to a observation vector.
    simpliest example:
    out = x; // observation = state
    this function is used to calculate the jacobian if an analitic function is not provided
    */
    return out;
}

lin::Mat<double> JF(lin::Mat<double> x, lin::Mat<double> u){ // state transition jacobian function -- OPTIONAL
    // n  is the size of the state matrix
    lin::Mat<double> out(n,n);
    /*
    Your code, that gives the jacobian of F(x, u) in respect to x.
    */
    return out;
}

lin::Mat<double> JG(lin::Mat<double> x){ // observation transition jacobian function -- OPTIONAL
    // m is the size of the observation matrix
    // n  is the size of the state matrix
    lin::Mat<double> out(m,n);
    /*
    Your code, that gives jacobian of G(x) in respect to x.
    */
    return out;
}


// n  is the size of the state matrix
lin::Mat<double> X(n,1); // state vector

// define here the initial state vector, example:
X = 0; // initialize state vector as 0


// m is the size of the observation matrix
lin::Mat<double> Z(m,1); // observation(sensor) matrix
lin::Mat<double> R(m,m); // sensor covariance matrix
// define here the sensor covariance matrix, example

R(0,0) = 9.3; // gpsx error
R(1,1) = 9.3; // gpsy error
R(2,2) = 8.3; // gpsv error


// n  is the size of the state matrix
lin::Mat<double> Q(n,n); // process covariance matrix
// define here the process covariance error matrix, example:
Q = Q.I()*0.005;

// b  is the size of the control matrix
lin::Mat<double> control(b,1); // control matrix

kalman::EKF ekf;

ekf.setState(&X);
ekf.setSensorNoiseCovariance(R);
ekf.setSensor(&Z);
ekf.setProcessNoiseCovariance(Q);
ekf.setControl(&control);
ekf.setF(F);
ekf.setG(G);
ekf.setJF(JF); // OPTIONAL
ekf.setJG(JG); // OPTIONAL 
ekf.init(a); // A value is used to set the Process covariance matrix initial value. a value between 1-100 is good.

while(true){ // kalman loop
    // sensor update example: 
    Z(0,0) = 1.1;
    Z(1,0) = 1.21;
    // control update example:
    control(0,0) = 12;
    ekf.predict_update(); // kalman cicle.
}


```


