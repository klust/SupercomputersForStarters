# Speed-up

Success in running on a parallel computer is often measured by looking at
the speed-up, i.e., how much faster does a code run on *X* processors compared
to on the smaller number $Y$ processors, with $Y$ typically one? 
So in a mathematical formula, 

\[S(X,Y) = {T(Y) \over T(X)}\]

with $T(X)$ the time on
$X$ processors and $T(Y)$ the time on $Y$ processors. Ideally $Y = 1$ but in 
practice for big problems it is not feasible to measure $T(1)$ because
the problem doesn't fit in memory or simply takes too much time to run.


