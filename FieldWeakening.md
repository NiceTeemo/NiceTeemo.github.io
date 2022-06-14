# PMSM Model
#### 电压方程
$$\begin{cases}
u_d=R_si_d+L_d\frac{di_d}{dt}-\omega_eL_qi_q\\ \tag{1-1}
u_q=R_si_q+Lq\frac{di_q}{dt}+\omega_eL_di_d+\omega_e\phi_m 
\end{cases}$$
#### 转矩方程
$$T_e=\frac{3}{2}p_n(\phi_mi_q+(L_d-L_q)i_di_q)\tag{1-2}$$

#### 电压圆和电流圆
三相SVPWM逆变器存在电压及电流限制，相应的电机电压和电流也不能超出这个阈值，假设系统的最大电压及电流分别为$U_{smax}、I_{smax}$，那么：
$$u_s=\sqrt{u_d^2+u_q^2}\leq U_{smax}\tag{1-3}$$
$$i_s=\sqrt{i_d^2+i_q^2}\leq I_{smax}\tag{1-4}$$

当电机转速较大，则定子电阻将远小于电枢电抗，忽略定子电阻部分，简化电压方程为：
$$u_s=\omega_e\sqrt{(L_qi_q)^2+(L_di_d+\phi_m)^2}\leq U_{smax}\tag{1-5}$$
在$i_d-i_q$平面中，根据上述电压限制方程，可以看出电压极限是以$(-\frac{\phi_m}{L_d},0)$为圆心的椭圆，随着$\omega_e$增大，电压极限椭圆往中心收缩。

# FieldWeakening
#### MTPA
将$i_d、i_q$用总电流$i_s$表示：
$$\begin{cases}
    i_d=i_scos(\theta)\\ \tag{2-1}
    i_q=i_ssin(\theta)
\end{cases}$$

根据（1-2）的电机转矩方程，将上式带入得：
$$
    T_e=\frac{3}{2}p_ni_ssin(\theta)(\phi_m+(L_d-L_q)i_scos(\theta))\tag{2-2}
$$
$\frac{3}{2}p_ni_ssin(\theta)\phi_m$为永磁转矩，$\frac{3}{2}p_ni_s^2(L_d-L_q)sin\theta cos\theta$为磁阻转矩。

表贴式永磁同步电机中，$L_d=L_q$，磁阻转矩为0，此时为了保证最大的电流利用率，只需要使用$i_d=0$的控制策略。
内置式永磁同步电机由于转子的凸极效应，交直轴电感不相等，从而存在磁阻转矩.同样的输出转矩，可以有很多种$i_d、i_q$组合，采用最大转矩电流比控制，能够使用最小的电流输出最大的转矩，提高电机效率降低器件损耗。

采用最大转矩电流比控制时，其中一种求解方法是利用此时$T_e$对$\theta$取极值的条件，构造$\frac{\partial T_e}{\partial \theta}=0$，结合式（2-2）得到：
$$\frac{\partial T_e}{\partial \theta}=\frac{3}{2}p_n\phi_mi_scos(\theta)+\frac{3}{2}p_ni_s^2(L_d-L_q)cos(2\theta)=0\tag{2-3}$$
求解上式，解得:
$$\theta_{mtpa}=arccos(\frac{\phi_m+\sqrt{\phi_m^2+8(L_d-L_q)^2i_s^2}}{4(L_d-L_q)i_s})\tag{2-4}$$

