
# pandas
```
df = pd.DataFrame(data)
x  = np.arange(df.iloc[0][x_header], df.iloc[-1][x_header], x_step)
df = pd.DataFrame(list(zip(x, y)), columns=[x_header, y_header])
mean = df[y_header].mean()
```  

```
def fit_sin_peaknull(df, x_header, y_header, half_period, x_res, isPeak=True):  
    x = np.array(df[x_header])
    y = np.array(df[y_header])
   
    x_max = df.loc[df[y_header].idxmax, x_header]
    
    guess_amp = (df[y_header].max() - df[y_header].min()) / 2
    guess_freq = np.pi / half_period
    guess_phase = -x_max * np.pi / half_period + np.pi/2
    guess_mean = df[y_header].mean()    
    
    optimize_func = lambda a: a[0]* np.sin(a[1] * x + a[2]) + a[3] - y
    p,cov,infodict,mesg,ier = leastsq(optimize_func, [guess_amp, guess_freq, guess_phase, guess_mean], full_output=1)
    est_amp, est_freq, est_phase, est_mean = p
    fit_coeff = {0: est_amp, 1: est_freq, 2: est_phase, 3: est_mean}

    ssErr = (infodict['fvec']**2).sum()
    ssTot = ((y-y.mean())**2).sum()
    rsquared = 1-(ssErr/ssTot )
    fine_x = np.arange(df.iloc[0][x_header], df.iloc[-1][x_header], x_res)
    y_fit = est_amp * np.sin(est_freq * fine_x + est_phase) + est_mean
    df_fit_fine = pd.DataFrame(list(zip(fine_x, y_fit)), columns=[x_header, y_header])
    v=0
    if isPeak:
        x_max_fit = df_fit_fine.loc[df_fit_fine[y_header].idxmax, x_header]
        y_max_fit = df_fit_fine[y_header].max()
        maxima = {'x': x_max_fit, 'y': y_max_fit}
        v=maxima
    else:
        x_min_fit = df_fit_fine.loc[df_fit_fine[y_header].idxmin, x_header]
        y_min_fit = df_fit_fine[y_header].min()    
        minima = {'x': x_min_fit, 'y': y_min_fit}
        v=minima
```

# matplotlib.pyplot  
```
            df.plot(x='Time', y=['V_1', 'V_2'], ylabel='ytypeV')
            plt.legend(bbox_to_anchor=(1.04, 0.5), loc="center left")
            df.plot(x='Time', y=['E_1', 'E_2'], ylabel='ytypeE')
            plt.legend(bbox_to_anchor=(1.04, 0.5), loc="center left")
            plt.show()
```

# numpy
np.sqrt()  
np.log10()  
np.pi 

# shutil
```
if not os.path.exists(target_path):
    os.makedirs(target_path)
copy2(originpath, targetpath)
move(file_path, target_path)
```