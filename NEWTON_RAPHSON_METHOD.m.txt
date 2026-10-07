%% NEWTON-RAPHSON METHOD
clc; clear;
fprintf('Newton-Raphson Execution Start\n');

% Anonymous function block
f_newton = @(x) x^2 - 4; 
df_newton = @(x) 2*x;

% 1. Local Function Solver Execution
root_local = newton_local_solver(f_newton, df_newton, 3, 1e-4, 50);
fprintf('1. Local Function Root:     %.6f\n', root_local);

% 2. Nested Function Solver Execution
run_nested_newton();

% 3. Recursive Function Solver Execution
root_rec = newton_recursive(3, 1e-4, 0, 50);
fprintf('3. Recursive Function Root:   %.6f\n', root_rec);

% 4. Private/Step Function Solver Execution
x_step = newton_step_private(3, f_newton, df_newton);
fprintf('4. Single Private Step:       %.6f\n', x_step);

% 5. Non-Visual GUI Step Execution (Runs the calculation without displaying a window)
run_newton_gui_silent(); 


%%  LOCAL FUNCTIONS 

% Local Solver
function x0 = newton_local_solver(f, df, x0, tol, maxIter)
    iter = 0;
    err = 1;
    while err > tol && iter < maxIter
        x1 = x0 - f(x0)/df(x0);
        err = abs(x1 - x0);
        x0 = x1;
        iter = iter + 1;
    end
end

% Nested Function 
function run_nested_newton()
    x0 = 3; tol = 1e-4; maxIter = 50; iter = 0; err = 1;
    f = @(x) x^2 - 4; df = @(x) 2*x;
    
    while err > tol && iter < maxIter
        execute_newton_step(); 
    end
    fprintf('2. Nested Function Root:      %.6f\n', x0);

    function execute_newton_step()
        x1 = x0 - f(x0)/df(x0);
        err = abs(x1 - x0);
        x0 = x1;
        iter = iter + 1;
    end
end

% Private/Step Function
function x1 = newton_step_private(x0, f, df)
    x1 = x0 - f(x0)/df(x0);
end

% Recursive Function
function root = newton_recursive(x0, tol, iter, maxIter)
    f = x0^2 - 4;
    df = 2*x0;
    x1 = x0 - f/df;
    if abs(x1 - x0) <= tol || iter >= maxIter
        root = x1;
    else
        root = newton_recursive(x1, tol, iter + 1, maxIter);
    end
end

function run_newton_gui_silent()
    newton_callback();
    function newton_callback()
        x0 = 3;
        x1 = x0 - (x0^2 - 4)/(2*x0);
        fprintf('5. Callback Step Output:      %.4f\n', x1);
    end
end
