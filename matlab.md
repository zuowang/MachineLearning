### "Eject DVD 1 and insert DVD 2 to continue"
This error will occur if the mounted ISO for DVD2 cannot be found at the mount point that MATLAB is searching for it.

To resolve this error, un-mount all MATLAB ISOs you currently have mounted on your computer, then mount *only *the DVD2 ISO. (Do not close out of the MATLAB installer while un-mounting/mounting your ISOs.) You should now be able to proceed with the MATLAB installer. 


### Linux下matlab断点调试
参考：http://cn.mathworks.com/help/matlab/ref/dbstop.html#inputarg_condition

几个重要命令： 
dbstop in file :在file的第一行设置断点 
dbstop in file at location：在file的location设置断点 
dbstop in file if expression：在expression为真时调试 
dbstop in file at location if expressione 
dbstop if condition: condition可选error | caught error | warning | naninf | …

dbclear all：清理所有断点 
dbcont: 断点后继续执行 
dbstep:断点后单步执行 
dbquit（或者：ctrl+c）:退出执行