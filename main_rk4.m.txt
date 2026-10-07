%% Main function (Acts as the executable entry point)
function main_rk4()
    clc; clear;
    fprintf('Runge-Kutta 4th Order\n');
    
    % 1. Test Local Solver
    f = @(x,y) x + y;
    [xf, yf] = rk4_local_solver(f, 0, 1, 0.1, 0.4);
    fprintf('Local Solver Result:     y(%.2f) = %.6f\n', xf, yf);
    
    % 2. Test Nested Function
    run_nested_rk4();
    
    % 3. Test Recursive Function
    y_rec = rk4_recursive(0, 1, 0.1, 0.4);
    fprintf('Recursive Solver Result: y(0.4) = %.6f\n', y_rec);
    
    % 4. Test Class Method (Using the external class file)
    solver = RK4SolverClass();
    y_class = solver.evaluateStep(0, 1, f);
    fprintf('Class Method (1 step):  y(0.1) = %.6f\n', y_class);
end

%% Local function
function [x, y] = rk4_local_solver(f, x0, y0, h, xn)
    x = x0;
    y = y0;
    while x < xn - 1e-10
        k1 = h*f(x, y);
        k2 = h*f(x + h/2, y + k1/2);
        k3 = h*f(x + h/2, y + k2/2);
        k4 = h*f(x + h, y + k3);
        y = y + (k1 + 2*k2 + 2*k3 + k4)/6;
        x = x + h;
    end
end

%% Nested function
function run_nested_rk4()
    x = 0; y = 1; h = 0.1; xn = 0.4;
    f = @(x,y) x + y;
    while x < xn - 1e-10
        execute_rk4_step();
    end
    fprintf('Nested RK4 Result:       y(0.4) = %.6f\n', y);

    function execute_rk4_step()
        k1 = h*f(x, y);
        k2 = h*f(x + h/2, y + k1/2);
        k3 = h*f(x + h/2, y + k2/2);
        k4 = h*f(x + h, y + k3);
        y = y + (k1 + 2*k2 + 2*k3 + k4)/6;
        x = x + h;
    end
end

%% Recursive function
function y = rk4_recursive(x, y, h, xn)
    if x >= xn - 1e-10
        return;
    else
        f = @(x,y) x + y;
        k1 = h*f(x, y);
        k2 = h*f(x + h/2, y + k1/2);
        k3 = h*f(x + h/2, y + k2/2);
        k4 = h*f(x + h, y + k3);
        y = y + (k1 + 2*k2 + 2*k3 + k4)/6;
        y = rk4_recursive(x + h, y, h, xn);
    end
end
