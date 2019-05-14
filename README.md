# Assignments
Hello, World
sc%>%filter(adm_rate>.3)%>%summarize(mean_earnings=mean(md_earn_wne_p6,na.rm=TRUE))
## # A tibble: 1 x 1
##   mean_earnings
##           <dbl>
## 1        34747.

> sc%>%summarize(mean_debt=mean(debt_mdn,na.rm=TRUE))
# A tibble: 1 x 1
  mean_debt
      <dbl>
1    11277.
> 
