The small-step operational semantics of Dart Kernel is given by an abstract machine in the style of the CEK machine.  The machine is defined by a single step transition function where each step of the machine starts in a configuration and deterministically gives a next configuration.  There are several different configurations defined below.

_x_ ranges over variables, &rho; ranges over environments, _K_ ranges over expression continuations, _A_ ranges over application continuations, _E_ ranges over expressions, _V_ ranges over values.

Environments are finite functions from variables to values.  _&rho;_[_x_ &rarr; _V_] denotes the environment that maps _x_ to _V_ and _y_ to _&rho;_(_y_) for all _y_ &ne; _x_.

An expression configuration indicates the evaluation of an expression with respect to an environment and an expression continuation.

Expression configuration | Next configuration
-- | --
<_x_, _&rho;_, _K_><sub>_expr_</sub> | <_K_, _&rho;_(_x_), _&rho;_><sub>_cont_</sub>
<_x = E_, _&rho;_, _K_><sub>_expr_</sub> | <_E_, _&rho;_, **VarSet**(_x_, _K_)><sub>_expr_</sub>
<_f(exprList)_, _&rho;_, _K_><sub>_expr_</sub> | <_exprList_, _&rho;_, **StaticInvocation**(_S : f.body_, _K_)><sub>_exprList_</sub>

An expression continuation configuration indicates the application of an expression continuation to a value and an environment.  The environment is threaded to the continuation because expressions can mutate the environment.

Expression continuation configuration | Next configuration
-- | --
<**VarSet**(_x_, _K_), _V_, _&rho;_><sub>_cont_</sub> | <_K_, _V_, _&rho;_[_x_ &rarr; _V_]><sub>cont</sub>
<**ExpressionList**(_tail_, _A_), _V_, _&rho;_><sub>_cont_</sub> | <_tail_, _&rho;_, **ValueApplication**(_V_, _A_)><sub>_exprList_</sub>

An expression list configuration indicates the evaluation of a list of expressions with respect to an environment and an application continuation.

Expression list configuration | Next configuration
--|--
<_&empty;_, _&rho;_, _A_><sub>_exprList_</sub> | <_A_, _&empty;_><sub>_cont_</sub>
<_E :: tail_, _&rho;_, _A_><sub>_exprList_</sub> | <_E_, _&rho;_, **ExpressionList**(_tail_, _A_)><sub>_expr_</sub>

An expression list continuation configuration indicates the application of an application continuation to a list of values.

Expression list continuation configuration | Next configuration
--|--
<**StaticInvocation**(_S_, _K_), _valList_><sub>_cont_</sub> | <_S_, _&rho;_[_formalList_ &rarr; _valList_]><sub>_exec_</sub>
<**ValueApplication**(V, _A_), _valList_><sub>_cont_</sub> | <_A_, _V :: valList_><sub>_cont_</sub>