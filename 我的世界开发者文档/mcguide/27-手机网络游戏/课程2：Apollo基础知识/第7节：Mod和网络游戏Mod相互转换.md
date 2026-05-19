# Mod和网络游戏Mod相互转换

​ 本教材介绍如何将Mod转换成网络游戏Mod，然后再将网络游戏Mod转换成Mod。与“Mod到网络游戏Mod入门”教材不同，本教材介绍的方法可以实现Mod和网络游戏Mod的相互转换，而“Mod到网络游戏Mod入门”只能实现Mod到网络游戏Mod的单向转换。本教材要求开发者已经申请到了开发机（按照“网络游戏入驻”流程申请），且会使用部署工具（参考“使用部署工具”），已经阅读“Mod到网络游戏Mod入门”

​ 网络游戏Mod绝大部分时间花在Mod开发上，服主可以使用Mod API开发调试，实现大部分客户端和服务端逻辑，还可以本地查看效果，大大提升开发效率。本地开发完成后再转换成网络游戏Mod，使用Apollo SDK实现网络游戏相关的功能。服主也可以把网络游戏Mod转换成Mod，使用本机开发调试。相互转换的条件是：不允许使用Apollo SDK中的API。下面我们以基岩版“入门脚本模板”为例说明他们之间的转换。

## Mod转化成网络游戏Mod

这里以入门脚本模板为例，介绍从入门脚本模板Mod转化成入门脚本网络游戏Mod的步骤。

1、入门脚本模板中，点击“更多-->转换为服务端Mod”，Mod名字为tutorialApolloMod，

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAgkAAAEOCAYAAAADj4TdAAAgAElEQVR4nO3df3BU9f3v8ReCgppuEolNzBDQSG9IwrS0ahnC1EAYv/2i3rnxLlf+6DYiVOkyNQiptX7bCHGnttZiSfgO8UcXI1/8AyfU3H6vpl4HAt4xDEXH2EsC+Y6GQpg0VDBhXRS/1nr/yP0cz9n9bH6QzSbA8zHjmN1zzud89vzzeZ3353MOkwr/5a0vBAAAEOOy8e4AAACYmAgJAADAipAAAACsCAkAAMCKkAAAAKwICQAAwGrKWDT6dMVs/euev+rQibPOd3NnXK3f3H2D/vmpQ559/TdnafZXp+mJV0843/1x/Vz9+KWjnuONh2+foRuypumH299z9o1t063jFzc5f1e/fEy3FWXo1oL0uP2KfvZ23HF31x+x9sHtj+vnaub0qZ5zzP7qNN2zMNv57vjpTwftYzL5b87SfbfmpOx8AICJafJlk7R+cbpebT+r9t7PPNtmZk7Rim+n6ck9Z/TJZ4nfhDAmIeH6rGlxg2tBzpVqOdI/5LH+m7M0c/pUvRSc43znHqyfePWE/Ddn6emK2U5QGIwZoB++fYa1PWlgoHd7umK2JHn64BZ7vAkY7nO88OZJJ/i42zehIlEA6fjFTSkNFQCAi9P6xen6p4Jp+k7+VD3y7x86QWFm5hT95r9do8yrLpPvysv046YPE7aR1JDwdMVs5y7d3MG/8OZJz121+bv65WO679Yc5y7cVAduK8pwBl3/zVm6rSgjbjDd9dYp7XrrVNL67a4EmIAgxVcXzLbY/sRWK6SB32l+6/HTn3r2P376U93x9cy4dtwhAwCA0Th47JzmfHWKZmZO0frF6fq3g1FJ0vdvSVPmVZfpeN/f9b8OfTxoG0kNCT/c/p4evn2G3vvbOc8g/sSrJ9Txi5viBt1db53yTDfEVgdiy+b+m7MUumtW3Hndg7Rh7tRnTp/qbDcDuDQwIJtB/IU3TzrfHT11zqkAdPziJlW/fEydvZ/opeAcVb98LC6cxN7xm4G+6GdvJ5yyaDnSr3sWZnumWCRp8ZwMvfDmSS2ekxH3ewAAGIm9752TNBAKZmZO0eqSr0iSstIm63jf3/VvB6POPokkfbrhhqxpeuXPfc6A756zdw/msSV1/81ZurUgPW7AN5+LfvZ2XAVh7oyr9VJwTsI7fvd5Yu/Sn3j1RNwg7f48d8bVkmQNJYY7aMQy35spC/fvfe9v5/RG5xk9fPsM55z+m7OcbYtjZjlir4lt/YRbbOUCAHBpMiFgdclXlJU2WZJ0Kvr5sAKClOSQ4L85S0dPeU/6z08dsi5kdLtnYbZeePOkXnjzpFOFcB9jqxRI0h1fz9QbnWfivr8+a9qg/bStNbi7/oh+c/cNTqA5fvpTa/gwoeGNzjP64fb3PMGi4xc36YU3T+qG/3/+wdZMvN7Rr9Bds5zj77s1R8+90Ru3n2nTHSbcVRlT7TDh6emK2UP+fgAAhiOpIcGs6jd30WYAu7UgPe6JAveaBDMIjnROfvGcDD33Rq+erpjtmSaQvlw3YJtuMNML7mmFQyfODvnkReyg716DIXnv8M1gbsRWTna9dUr33ZrjTM+Y70xFwbRx/PSnnt9ljjP7HT/9qae68npHv+67NWewywYAuEQsmj1N378lTVlpk3Uq+rmkgemG79+SJkmpnW4wJfy5M67Wj8qucwZV9yBmHtEzUwdmIHaX7s3dunsAjn3U0QQK087Dt89wpjjcCxFt0w2v/LlPLwXnOMHEhAZzHvfxkuKmFMw6A9t0igki7mmKRE8rPPdGr+67NUc3ZJ0b1pMfbp29n6gg58oRHQMAuHSYgDAzc4qzBkH6co3CcIJCUkNC7ABrSuWL52So5Ui/3vvbuYTP8LvXCJgg8dwbvdanGObOuFr3LMzW3fVHPMebbWZO3qwrsKl++Zj1cUP337b3Nfxx/dy4aZORPALptuutUwrdNUszp0+1Tk2Y7ba1C4dOnNWhE2cVumuW/DdnOdeJKgIAQJJuvXGaZmZO0ef/+EINf4rq/7w/EAY+iH6uX/7XazQzc4r+xzfTUhcSzAAbW0kwd+6SPHftNmZO3axliH3RknnCIdF7Bu74eqbn70R36LO/OjBvP3P6VM2dcXXci5/M0wyx54itMkgjfwTSzb2GwcY8JeGuZrinNapfPqbQXbOcygVPRwAAJOnJPWfku/Iy/c//+7ETECSpvfczPfLvH2rNd9L181cSvyNBkiYV/stbiV+1dJ5iQ4L7e/ejhKby4F6fEPuYoZmGeKPzjLPYz7ag0D01Ydowix/NgsS764/oR2XX6daCdGfhofTlC4yee6NXobtmeaoLps+Gu0IgeadB3OsL3OFmqLdCAgAwEY1JSAAAABc+/oEnAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVlOKOn853n0AAAATEJUEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUiwCIfDuv3228e7GwAAjKuLIiSUlZWpsbHRum3nzp0jHvDffPNNLV++XFOnTk1G9wAAuCBNSdWJGhoalJaWpmXLlsVtKysr05o1a9Te3q4NGzYk7Zw7d+7U5MmTtXLlSq1cudKz7ciRI/r5z3+ulStXJgwRL774oufzvn37tGXLlqT1DwCAiSxlIUGSotGoampq4oJAeXm5otFo0s5TXV2tb3zjG9ZB/Tvf+Y5WrVqlUCjkfGcCw1BtAgBwKUlpSDh27JiKi4vjvs/NzVV7e3tSzlFdXa3Zs2fre9/7nn71q1/p9ttv16uvvipJWrlypUpLS3X//ffr008/lSRt27ZNkvTb3/5WeXl51jaHEyIAALjYpDQk9Pb2atasWQoGg6qvr5ck1dTUWANCMBjUkiVLnM+xUxFm+iKWu0Kwbt067dy5UzfeeKPy8/N1+eWX65577rH2bd26dXHfmYrEE088MfwfCQDARSLlCxcPHDig+fPnO5+Li4u1b98+zz4mICxbtsz5r7i4WMFgUJJUV1enSCTibBusCnHo0CGVlpaqq6tLP/rRj4bdTxMQ7r33Xn300Ucj/JUAAFz4UlpJkKT6+notWbJEwWBQOTk56unp0Z49e1RaWursU1hYGDfwt7e3q7CwUNLA9MTWrVudbfv27YubxoidPigtLfWcw0whhMNhpaenJ+zv888/7/x99uxZz1QFAAAXs5SHBOnLAd/n8+nAgQNJbduEg2g0mnAtwcqVK5Wfny9JWrVqlXX7t771rRFVHgAAuNiMy3sSNmzYoNzcXEly1ia4HT58OK4yUFxcrMOHD0saeEqivLzc2eb+e926dVq2bJnef//9seg6AACXjHGpJEhST0+P+vr6rNvq6+uVk5PjeUHS7t27nUCxYsUKNTY2Otvb29ud0OE2Z86chC9ZOnLkyGh/AgAAF7VJfr//i/HuxFiorq7W1KlTB51uiN3mXsfAi5MAAJe6izYkAACA0bko/u0GAACQfIQEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVkl94+JfrvmnZDYHAADO0/Uf/u9Rt5HUkPDxtTcp650nktkkAAAYoVPffFiaaCFBkiZ/8kGymwQAAOOANQkAAMCKkAAAAKwICQAAwIqQAAAArAgJAADA6qIPCRs3btSWLVuG3G/Lli3auHFjCnrkPddw+5cK27dv1+rVq8e7GwCACeKiDwmjsWjRIjU1NU2YgXPjxo2D9mf79u3avn17insFALhYpTwkjPSOfbR3txs3btQDDzxwXscuWrRIkUhECxcuPO/zJ1ui/kyUIAMAuHhQSRhEfn6+tm3bJp/Pp0WLFo13dyRJZ86ckRQfCubOnauurq7x6BIA4CKV9DcuDmb79u3y+XzKy8tTU1OTNm/eLL/fr9OnTzvVhdWrV2vhwoWqqKhQU1OTJGnp0qVaunSpysvLJQ1UI/Ly8px2N2/erL1790qSmpqa1NbWpnnz5qmtrU2SNH36dKeasHr1ai1dutQ51rQZywzCe/fuld/vl9/vd85hztPc3JywrdjztLW1DauCMpzjurq6NHfuXOfzokWLlJ6erl27dik/P9+zr7mG0kAVoqKiwvm8ceNGzZs3b8g+AQAuTSmtJFRUVKi7u1ttbW0qLy/3DLo25eXlikQiam5u9gQEs628vFzNzc168MEHPcdNnz5d5eXl1kF57ty5zrHd3d0JB273nfmhQ4c8ocRYuHCh01ZbW5uzHsAM9GZbeXm55s2bN+SUwHCP27hxo/Ly8pzqht/vt1YRTJAxbZ05c8a5fqtXr9a8efOcbZs3bx60bwCAS88FN92Ql5enXbt2OZ+feeYZRSIRz0Dq3h7rgQce0Pbt29XU1KS8vDxNnz494Xn++te/OueQ4kv827Ztc/7euHGjfD6fpIGAYaoYRltbm+fu32Ykx3V3d8vv92vRokXKy8uLCzurV69WJBJx+i4NXBcTdmLPtXfvXkUikUH7BwC4tFxwIWE0zNMB27Ztc+7+E+0nDUxzNDU1OSX7wRYwpnrNghnwFy1apO7u7pSeGwBwaRj3kHD69GnPPPpQTxKYO2hj9erV8vl8njvmRKZPn67u7m5nmiPRfHx+fr66u7s9Zf/NmzfHLWB098Pv9zuD9aFDh+Lanjdvng4dOjRo/0Zy3N69e9Xd3a158+ZZKyfPPPOMfD6fp/rh7uPp06c95zLXEQAAI6ULF6WBgdDcoW/evNm5uzd3621tbZ7Q0NXV5Vm46J4uMBItPoy1a9cuPfjgg55zxU43LFq0SD6fzzOVINkXMJ4+fdppy70o8JlnntF1113n6WNzc/OQQWakxx06dEjp6ekJ13Zs3rxZDz74oLMQsru721nAaV7iZM7V3d3NdAMAwGOS3+//IlmNdRQ8ouzWHyeruQnNhJyhFl8CAJBqJ0t+o6LOX466nXGfbgAAABMTIQEAAFilfE3CxWK46yAAALhQUUkAAABWhAQAAGBFSAAAAFZJX5NwLnt+spsEAADjIKkhIbf3Fenqq5LZJAAAGKGre19JSjtJDQkZZ/6czOYAAMA4Yk0CAACwIiQAAAArQgIAALAiJAAAACtCAgAAsCIkAAAAK0ICAACwIiQAAAArQgIAALAiJAAAAKuk/wNPiTQ0NGjFihVqbGzUsmXL4rbX1NSouLhY7e3t2rBhg3W7JOs2o7GxcdA+bN26VXv27BlhzwEAuDSlLCTEMqHA6OnpsYaHkYhGo1qxYoV1W0NDw6jaBgDgUjNuIUGSdu/erfr6+rjvYwOEW2y1wF15SEtLG7KaAAAAhmdcQ0IisVMKZWVlWrNmjSQNWm2gkgAAQPJM8vv9X6TiRLYKgK1aYFs30NDQoGPHjkmSMjMzVVlZOXYdBQAAklJYSYi9y6+pqVFPT4/6+vqcykFDQ0NcQKirq9OBAweUk5MjSTp8+LBqamo81YaGhgalpaUNuy+jXfsAAMClIGUhwb1eYNmyZcrMzFRfX59mzZolaWBKIRKJeI6pq6tTX1+f6uvrnacb6uvrVVdX5wkKJnwEg0Hl5OQM+gQEAAAYnpRXEtxrA3p7e5WZmalgMKj58+dr+/btzjYzxWAb8CsrK1VXV6e6ujrP1MP8+fMHXbyYaKEkAACIl5KQYKsS5ObmOgN8Y2Oj2tvbnakGM8VgtrmZz1u3blV5ebknKCRatGjaBAAAw5eSkFBaWqq+vj7nczAYVE9Pj6SBikE0GlVxcbGCwaDq6+s91QFz5297mdJIXoyUm5urpqamUf0OAAAuJSkJCZmZmZ4BurCwUH19fWpsbPQ8zdDQ0KDCwsJRP70QDAa1ZMkSz3fuSgUAABhayh6BBAAAFxb+gScAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWSX1PwuWXX57M5gAAwHn67LPPRt1GUkOCz+fTmTNnktkkAAAYofT0dJ0+fXrU7ST9jYunTp1KdpMAAGAE0tPTk9IOaxIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYJf0RyOHatWuX3n33XT322GOD7mPU1dVp3759cfuUlpaqsrLS+ez3+5PbUQAALlEpryQ8//zz2rVrl7q7u4fc791335Xf79drr73mCQJulZWVeu211+T3+/Xuu+/q+eefH4tuAwBwyUl5SLj33nuHvNsvLS2Vz+dzqgzPPvusIpGI7r//fs9+999/vyKRiJ599llJ0mOPPSafz6fS0tKx6TwAAJeQCbkmoaCgIK7ScObMGeXk5Hi+y8nJ0dGjRz3fRSIRFRQUjHkfAQC42E3IkBAbBhK55pprxrgnAABcuiZkSOjt7R3Wfh9++OEY9wQAgEvXhAwJUvw/TpGenm4ND7HVBJ/Pp87OzjHtGwAAl4IJExJKS0u1a9culZaWOgsRzUJF8/9nn33Ws99jjz2mvLw8Z6Hio48+qu7ubuujkgAAYGTG7T0JQ2loaFBlZaW++93vSkr8/gPzeGRlZaUikYjuvffeVHYTAICL1iS/3/9FshqbPn263n///WQ1BwAAzsONN96o06dPj7qdCTPdAAAAJhZCAgAAsCIkAAAAK0ICAACwIiQAAAArQgIAALBK+nsSLruM3AEAwMUgqSHh3Llzuu6665LZJAAAGKFz584lpZ2khoSzZ88mszkAADCOmBsAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYJX0f+DJpqysTBUVFVqxYoV1e0NDg9LS0iRJy5YtkyTV1NSouLh4yLbN/o2Njc53u3fvVmFhoXJzcxPubzQ2Nmrr1q3as2fPoOdx99GcIycnx9PHaDTq/MZgMKglS5Y4bdfV1ampqck5j/u8Zt9Y0WhUBw4cUGFhoSorK9XQ0JDwGgIAkGwpCQl79uxRaWmpampqtGHDBs+ALinhIN3e3q4NGzZIGgga5eXlqqysdLY3NDQ4f5sBuqamJmG77v0lqa6uTpK0Zs0arVmzJu78sce7A4ytj+726+vrlZOTo4qKirjf1tjYqPb2ds++9fX1amxs1LJlyxQMBp1gEAwG4/oFAEAqpCQkSNKGDRvU0NCgsrIyz918Q0PDkHfxyeKuBJiAIMVXF8w22+Bu9Pb2SpKKi4ud76PRqGd/Ex5ixZ4PAICJaJLf7/8ilSccahrBXREY7XRDU1OTSktLnXbMXb+pBJhBvLGxUbt371ZnZ6fWrFmj3bt3q76+fsjfYdqInbKoq6uzTnXYtLe3a9asWZ4A49bT0yNJTDcAAFIuJZUEM5/vLs27tyUa+EY73SDZ7+bd35WVlUmSlixZYl0XIA0ebEw4MVMW7nUJZnt7e7syMzOVm5urnp4ez29w98Os24idbigsLLSeGwCAsZSSkGAGvpycHEnxg667XO8eYE1J/3zZ1hps3bpVFRUVzp17NBq1TjeYPtkGdffALylue6KFmIcPH3aON0z75eXlOnbsWNxvGKqiAQDAWEnZmoRYsVUFcyftZru7j130aKSlpTnbTLgwiwPd0wp79uyJW2tgAozZL3bQj50+cIeKYDDo6VNs0DEyMzO1b98+7dmzJ27gN4sTE61hSLTmAQCAsTRuIWEomZmZnrUB5zPdsG/fPqeSUFNT43miIPaRRik+gJh1BmY9gNnfrGGQ5Akxpg+2tiV7ZcM2BVNfX6+amhoFg0HV19ez0BEAMC7GLSS4nwow3HfKPp9v2KV2s67AZvfu3WpsbIy7w3f/3dDQoO3bt8c9LhlbcRjuI5CxlYRgMKj58+crLS0t4YBvW+xYXFxsDSEAAKRCSkKC+2VBZWVlCcvq7v1HorS01DqfL8lZB5GWlqaysjLPwF9WVuY8zRAbCGyVgJE+Aml+t3twN/vGhoXYKY6amhr19vayJgEAMG5SEhIKCws9jyraXlxkmEf+Dhw4YN1uBnbpy0E5MzNTTU1NTpl/3759Kiws1Jo1a9TT0+M5d0VFhQ4cOOAM3mabu11JnqkJY7iVBNMP26LI2Ec2zW+whZLYSoLZn2oCACAVUv6eBAAAcGHgH3gCAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGA1JRUnCYfDozp+1apVSerJhYnrBwAYD1QSAACAFSEBAABYpWS64Xx0dXUpPz//vI+vqqrSyZMntWPHjoTbi4qKhmwnGo1q7dq1npJ/S0uLCgoKlJubG7f/RCntj+T6BQIB3XLLLVq7dq1KSkq0fPlyrV27VpJUUlKipUuXqrq6esh2zL79/f2aOXOm04akC+76AQAmaEj4wx/+oOeee0533nmnfvCDH4zZeVpaWhKGiFgmLFRVVTnfhcNhtba2Op9ra2uT3sfzMdLrZ65BKBRSdXW1FixYoKqqKm3atEkLFixQbm6uM8hHo1EdP37cE7B6eno8IWLTpk0qKSlRbW2tExQupOsHABgw4UKCGeAkqa2tTWfPnh3nHg1fWlraeHfhvK/fjh07FAgEPHf8tbW1ikQig97dm+qBJLW2tmr58uUKhUJOlcCEjeGYCNcPAPCllIaEvr4+ZWZmJtzuHuBmzJihxx9/XD6fb8h23YOSW1FRkRYvXuz5rqOjwxm0srOzh3xywNwBp6WlOfuePHnS2e6etujo6Biyr6MxVtevtrZWBw8edCoK2dnZ2rRpk0KhkHw+n+cauX/jpk2blJ+fr/7+fs8+Bw8ejJuemAjXDwAwMikLCR0dHdqwYYPuvPNOBQIBTZ482bPdNsANNiC62ebLQ6GQOjs7B51OOHnypOcuN3Y+3s1WLpc07Lvk0RrL67d27VprqT/2uobDYe3fv1+tra2qra1VSUmJsrOzPdextrbWueZDTTdIqbt+AICRS0lI+Pzzz/W73/1O586dU2Njo44ePaqf/vSnmjZtmqTRDXCJ+Hw+FRQUjLrvg1m1alVcKT52nj0ZUnH9zGBu7uzD4bBnrUEoFFJHR4fz23bu3Knly5dLkvbv3x/XXlVVlY4fPz7oOVN1/QAA5yclj0BOnjxZjz/+uG666SZJ0ttvv61169bpxIkTYxIQAoGAIpGIpIHqQDKYcrl7wV5HR4c6Ojo8g91YDHCpuH6hUEiBQEDSwO9qaWlxphrC4bByc3NVVFTkVBxaW1udEOD+zZFIRIFAQDNnzvRUCcbz+gEAzk/K3pMwbdo0VVdXa9myZZKkEydOaN26dUkPCJK0ePFiNTc3q7m52VlUFysjI0NdXV3DbjMajWrVqlWeefP9+/c7g15VVdWYzqmP9fXz+Xzq6upSRkaGs2bg+PHjzgBufr97KiYjI0NpaWmeINbf36/Fixdr586dnvbH+/oBAEYupS9Tmjx5su655x6tX79eV1xxhc6dOycpuQEhHA6rpaVFra2tam1tVX9/v0KhUNx+ubm5w75rHawa0dLSonA4HHfnPBbG+vq1trY6YWEogUBAPp9PLS0tniBWVFSkaDTqXFuzdiGRVF4/AMDIjMsbFxcvXqxf/epXmj59elKnGExAcC9WNAOPe/V9IBBQT0+PtR0zTeG2YMGChPPr2dnZkhR3Rz2Wkn39SkpKnGkCaSAsmN81WB927tzpXGv39Y9EIqqqqnKmfSba9QMADM+4vSfha1/7murq6vT555+POiCYR/USPc9fXV2tkpISZzGeJDU3NzvbA4GA86ikreSdkZGh5uZm1dbWKi0tTfv371dBQYFWrVqlnp4e57zhcDjh0xHJlszrt2DBAvX39+uWW27RwYMHnUAVDoedx0vdoSoUCjnVGmng+obDYc/1r62tVVFRkfN2xYl2/QAAQ5vk9/u/GOuT8K8Yjg7XDwAwHvgHngAAgFVKKgkAAODCQyUBAABYERIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYERIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYERIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYERIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYERIAAIDVlPHuAACMtY6CR8a7C5eEos5fDmu/p556aox7Aklav379qNsgJAAAUi4ZAxgSS1YQY7oBAABYERIAAIAVIQEAAFgREgAAgBUhAQAAWBESAACAFSEBAABYERIAAIAVL1MCgBR6ceVM7TjQp+b2j5zvlhZ/RZVlWVq65ahn34duu1YzMi/X2pd6nO+aH7hBdXtOeY7H+QuFQurv79emTZvivu/s7NSOHTuc76qqqpSRkaHq6mpVVVWpqKjI2mZLS4vnuAsZIQEAUuiaqyfHDfBzc6fpP05+OuSxD912rTKunKxH78jWo3dkS5Iee+UkgWEUqqurFQqFVFtbq7Vr10r6MjjEDvSbNm1SIBBQKBRSdXW1tb1QKDTmfU4lQgIApMCLK2cqP+sKSdL+n8yWJP3pLx/r29df5exjvv/9O2dUNidNGVdOdo793rbjmpd3pRb8+j1JA4FhXt6VBIRRqK2tVVpamvM5HA47f+fm5jqfV61apZKSEi1dujQuHAQCAWVnZ8dVIi4WhAQASIHvbTuu2rtzdaLvMz35+geebft/MtsZ/I0nX//AM91ggoJRNictbnoCI2MqB8ZwBvxAIKDFixfHfR8Oh9XT02M54sJGSACAFMlKm6I/tn/kDPjND9zgVAtMFUGS+j/53BMAHrrtWuVnXeHZx31MbMDA2NmxY4dnGiIcDqujo8MJFkw3AABG7KHbrtWp6N893y3dctS6kNFtYDriY/3pLx87VQj3MbHBAcMz2MJD97SDJPX09Ki5uXmUMTUAAAKGSURBVDluv9raWnV0dIxJ/yYKQgIApMCMzMv17euvctYg7P/JbP3+nTPKz7rCsxBR8q5J+NNfPtbal3pUe3fueHX9omSbUqitrVUkErEuSiwpKfF8DoVCOnjwoCQpOzs7bv+LBe9JAIAUWPtSjxb8+j099spJdZ36Ty349Xt68vUPtODX7zn//f6dM+r/5HM9+foHWrrlqH7/zhlJUu3dufr29Vfpv38zXft/MtsJFqaK0PzADeP50y4KVVVVikQi6uzsVCAQGHTf2tpa69MPFyMqCQCQAu71B9JAJeFPf/lY/yV7qv7j5Kc60fdZwsWI7vckPHTbtSqbk6Y9R6JxCyBxfgKBgGbOnOl5BDIQCFhDQCgU0vHjxy/apxliERIAIAXM4L+0+CsKzM90nlRYWvwVPXpHtr59/cAjkYN5ceVMXXP1ZGctQ+3duZ4AgZEzTyW4n3Qw704Ih8NatWqVZ38zFeF+yqGlpcWzT1dX1xj3OnUICQAwjprbP1Jz+0dOWHjotmv15OsfOJWH379zxvO3qR6YRyr3/2S2uk79p+fxSAwtFAopNzc3LgQYJgyYEBG7cNH9lEMgEHAWO0ajUbW2to5hz1Nrkt/v/2K8OwEAY6mj4JHx7sIloajzl8Pa76mnntL69evHuDeXtmRdYxYuAgAAK0ICAACwIiQAAAArQgIAALAiJAAAACtCAgAAsCIkAAAAK16mBABIuaeeemq8u4Bh4GVKAADAiukGAABgRUgAAABWhAQAAGBFSAAAAFaEBAAAYEVIAAAAVoQEAABgRUgAAABWhAQAAGA1paPgkfHuAwAAmICmzDj4iP7xj3/oiy8G3s5s/g8AAC5NkyZNkiT9P+fN7AkfcKYuAAAAAElFTkSuQmCC)

在服务端Mod中可以找到tutorialApolloMod

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan8.ed307da4.png)

2、打开tutorialApolloMod所在目录，修改tutorialApolloMod脚本根目录名，将tutorialScripts改为tutorialScriptsDev

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan7.a4c06e5c.png)

由于改了根目录名，所以需要将tutorialApolloMod/tutorialScriptsDev目录中，所有使用“tutorialScripts”的python包都改为“tutorialScriptsDev”。在tutorialScriptsDev目录下搜索tutorialScripts，发现只需改modMain.py文件，替换之后核心代码如下：
```python
    class TutorialMod(object):
        ...
    
        @Mod.InitServer()
        def TutorialServerInit(self):
            #serverApi.RegisterSystem("TutorialMod", "TutorialServerSystem", "tutorialScripts.tutorialServerSystem.TutorialServerSystem") #包含了tutorialScripts的python包，需要改名为tutorialScriptsDev
            serverApi.RegisterSystem("TutorialMod", "TutorialServerSystem", "tutorialScriptsDev.tutorialServerSystem.TutorialServerSystem")
    
        @Mod.InitClient()
        def TutorialClientInit(self):
            #clientApi.RegisterSystem("TutorialMod", "TutorialClientSystem", "tutorialScripts.tutorialClientSystem.TutorialClientSystem")#包含了tutorialScripts的python包，需要改名为tutorialScriptsDev
            clientApi.RegisterSystem("TutorialMod", "TutorialClientSystem", "tutorialScriptsDev.tutorialClientSystem.TutorialClientSystem")
    	...
```
3、ModMain.py中注释不相关的代码。客户端ModMain.py文件只包含客户端的入口和退出函数，需要屏蔽TutorialServerInit和TutorialServerDestroy函数
```python
    @Mod.Binding(name = "TutorialMod", version = "0.0.1")
    class TutorialMod(object):
    
        # 类的初始化函数
        def __init__(self):
            print "===== init tutorial mod ====="
    
        # # InitServer绑定的函数作为服务端脚本初始化的入口函数，通常是用来注册服务端系统system和组件component
        # @Mod.InitServer()
        # def TutorialServerInit(self):
        #     print "===== init tutorial server ====="
        #     # 函数可以将System注册到服务端引擎中，实例的创建和销毁交给引擎处理。第一个参数是MOD名称，第二个是System名称，第三个是自定义MOD System类的路径
        #     # 取名名称尽量个性化，不能与其他人的MOD冲突，可以使用英文、拼音、下划线这三种。
        #     serverApi.RegisterSystem("TutorialMod", "TutorialServerSystem", "tutorialScripts.tutorialServerSystem.TutorialServerSystem")
    
        # # DestroyServer绑定的函数作为服务端脚本退出的时候执行的析构函数，通常用来反注册一些内容,可为空
        # @Mod.DestroyServer()
        # def TutorialServerDestroy(self):
        #     print "===== destroy tutorial server ====="
    
        # InitClient绑定的函数作为客户端脚本初始化的入口函数，通常用来注册客户端系统system和组件component
        @Mod.InitClient()
        def TutorialClientInit(self):
            print "===== init hugo fps client ====="
            # 函数可以将System注册到客户端引擎中，实例的创建和销毁交给引擎处理。第一个参数是MOD名称，第二个是System名称，第三个是自定义MOD System类的路径
            # 取名名称尽量个性化，不能与其他人的MOD冲突，可以使用英文、拼音、下划线这三种。
            clientApi.RegisterSystem("TutorialMod", "TutorialClientSystem", "tutorialScripts.tutorialClientSystem.TutorialClientSystem")
    
        # DestroyClient绑定的函数作为客户端脚本退出的时候执行的析构函数，通常用来反注册一些内容,可为空
        @Mod.DestroyClient()
        def TutorialClientDestroy(self):
            print "===== destroy hugo fps client ====="
```
服务端ModMain.py只包含服务端的入口和退出函数，屏蔽TutorialClientInit和TutorialClientDestroy函数。

4、转换完成，部署后点击“大厅服”中“大厅服4000”可以查看服务器日志，日志包含“===== init tutorial server =====”，它是执行服务端modMain.py文件中TutorialServerInit函数打印出来的

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan1.6fff3d1b.png)

## 网络游戏Mod转化成Mod

这里以入门脚本模板为例，介绍从入门脚本网络游戏Mod转化成入门脚本Mod的步骤。

为了查看转换效果，分别在客户端Mod和服务端Mod中新增一条日志。在服务端Mod的tutorialServerSystem.py文件中新增一条日志
```python
    class TutorialServerSystem(ServerSystem):
    
        # ServerSystem的初始化函数
        def __init__(self, namespace, systemName):
            # 首先调用父类的初始化函数
            super(TutorialServerSystem, self).__init__(namespace, systemName)
            print "===== TutorialServerSystem init ====="
            print "===== server test init ====="
```
在客户端Mod的tutorialClientSystem.py文件中新增一条日志
```python
    class TutorialClientSystem(ClientSystem):
    
        # 客户端System的初始化函数
        def __init__(self, namespace, systemName):
            # 首先初始化TutorialClientSystem的基类ClientSystem
            super(TutorialClientSystem, self).__init__(namespace, systemName)
            print "==== TutorialClientSystem Init ===="
            print "===== client test init ====="
```
**转换前需要确保没有使用任何Apollo SDK中API** ，下面介绍转换过程：

1、创建一个空白的AddOn（针对没有使用地图的网络游戏），删除behavior、resource相关目录，删除world_behavior_packs.json和world_resource_packs.json

2、将网络服behavior_packs、resource_packs、worlds/level 目录下所有内容拷贝到入门Mod所在目录下：

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan4.128bffd6.png)

3、将developer_mods/tutorialApolloMod 下内容拷贝到入门Mod的tutorialApolloModBehavior目录下

4、转换完成，然后开发测试，发现打印了“===== server test init =====”和“===== client test init =====”日志

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan3.87963153.png)

5、tutorialApolloModBehavior目录下包含两个脚本目录：tutorialScripts和tutorialScriptsDev。要求服主在tutorialScripts中开发客户端逻辑，在tutorialScriptsDev中开发服务端逻辑。再次将Mod转换成网络游戏Mod时，只需将tutorialScriptsDev到拷贝网络服developer/tutorialApolloMod目录，将tutorialApolloModBehavior拷贝到网络服behavior_packs目录，将tutorialApolloModResource拷贝到网络服resource_packs目录，将world_behavior_packs.json、world_resource_packs.json拷贝到网络服worlds/level 目录

## 网络游戏Mod和Mod相互转换

上面是处理不带地图的网络游戏，下面介绍如何将带地图的网络服Mod转出Mod，以入门脚本网络游戏Mod（新增了地图）为例做说明

1、新建一个空白的**地图Mod** （针对有使用地图的网络游戏），删除Mod所在目录中behavior_packs、db、resource_packs目录

2、将网络服Mod中behavior_packs、resource_packs目录拷贝到Mod所在目录，将level目录下全部内容拷贝到Mod所在目录，将developer_mods中脚本根目录（developer_mods/tutorialApolloMod/tutorialScriptsDev）拷贝到任意一个behavior_packs的Mod里面，也即将网络服developer_mods/tutorialApolloMod/tutorialScriptsDev拷贝到Mod的behavior_packs/tutorialApolloModBehavior 目录中

![](/dev/mcmanual/mc-dev/assets/img/image-zhuanhuan2.2418c7e9.png)

执行上面步骤后就完成了网络游戏Mod到Mod转换，完成开发调试后，可以进行逆向转换，也就是将上面Mod转换成网络游戏Mod。具体过程是：

1、将behavior_packs、resource_packs拷贝到网络服tutorialApolloMod目录下，将behavior_packs/tutorialApolloModBehavior/tutorialScriptsDev拷贝到tutorialApolloMod/developer_mods目录

2、将db、level.dat、level.dat_old、levelname.txt、world_behavior_packs.json、world_resource_packs.json拷贝到worlds/level目录下

进阶

20分钟