% SECANT METHOD
%Function used:f(x)=x^3-x-2, df(x)=2x, x1=2, tol=1e-4
%% Main function
function main_secant()
    clc; clear;
    f = @(x) x^3 - x - 2; 
    
    % 1. Local Function
    root_local = secant_local_solver(f, 1, 2, 1e-4, 50);
    fprintf('Local Function Root: %.6f\n', root_local);
    
    % 2. Test Nested Function
    run_nested_secant();
    
    % 3. Test Recursive Function
    root_rec = secant_recursive(1, 2, 1e-4, 1, 50);
    fprintf('Recursive Function Root: %.6f\n', root_rec);
end

%% LOCAL FUNCTION
function x1 = secant_local_solver(f, x0, x1, tol, maxIter)
    iter = 0;
    err = 1;
    while err > tol && iter < maxIter
        x2 = x1 - f(x1)*(x1 - x0)/(f(x1) - f(x0));
        err = abs(x2 - x1);
        x0 = x1;
        x1 = x2;
        iter = iter + 1;
    end
end

%% NESTED FUNCTION
function run_nested_secant()
    x0 = 1; x1 = 2; tol = 1e-4; maxIter = 50; iter = 0; err = 1;
    f = @(x) x^3 - x - 2;
    
    while err > tol && iter < maxIter
        execute_secant_step();
    end
    fprintf('Nested Secant Root: %.6f\n', x1);

    function execute_secant_step()
        x2 = x1 - f(x1)*(x1 - x0)/(f(x1) - f(x0));
        err = abs(x2 - x1);
        x0 = x1;
        x1 = x2;
        iter = iter + 1;
    end
end

%% RECURSIVE FUNCTION
function root = secant_recursive(x0, x1, tol, iter, maxIter)
    f0 = x0^3 - x0 - 2;
    f1 = x1^3 - x1 - 2;
    x2 = x1 - f1*(x1 - x0)/(f1 - f0);
    if abs(x2 - x1) <= tol || iter >= maxIter
        root = x2;
    else
        root = secant_recursive(x1, x2, tol, iter + 1, maxIter);
    end
end

%% PRIVATE / UTILITY FUNCTION
function x2 = secant_step_private(x0, x1, f)
    x2 = x1 - f(x1)*(x1 - x0)/(f(x1) - f(x0));
end

%% CALLBACK FUNCTION
function run_secant_gui()
    fig = uifigure('Name', 'Secant App');
    btn = uibutton(fig, 'Text', 'Compute Step', 'Position', [100 100 120 40]);
    btn.ButtonPushedFcn = @secant_callback; 
    
    function secant_callback(src, event)
        f = @(x) x^3 - x - 2;
        x0 = 1; x1 = 2;
        x2 = x1 - f(x1)*(x1 - x0)/(f(x1) - f(x0));
        uialert(fig, sprintf('Next step: %.4f', x2), 'Callback Triggered');
    end
end
