Expression configuration | Next configuration
-- | --
<_x_, _&rho;_, _K_><sub>_expr_</sub> | <_K_, _&rho;_(_x_), _&rho;_><sub>_cont_</sub>
<_x = e_, _&rho;_, _K_><sub>_expr_</sub> | <_e_, _&rho;_, **VarSet**(_x_, _K_)><sub>_expr_</sub>

Expression continuation configuration | Next configuration
-- | --
<**VarSet**(_x_, _K_), _V_, _&rho;_><sub>_cont_</sub> | <_K_, _&rho;_[_x_ &rarr; _V_]><sub>cont</sub>
