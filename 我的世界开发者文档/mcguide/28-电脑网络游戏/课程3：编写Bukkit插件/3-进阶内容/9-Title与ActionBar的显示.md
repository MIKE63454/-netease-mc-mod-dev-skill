# Title与ActionBar的显示

> _主观上，在1.12.2后我们发送ActionBar和Title._  
>  _Title只需要通过Player中的sendTitle方法._  
>  _而ActionBar发送方式则是 Player 对象封装了内部对象 spigot()，其中里面则有静态方法 sendMessage 通过传入ChatMessageType枚举参数，便可以发送ActionBar_  
>  _用Spigot的sendMessage得将内容转换成TextComponent_

但是Bukkit就没有能够直接发送ActionBar的发送方法. 所以我们就要通过NMS底层去自行封装一个方法.

进阶

3分钟