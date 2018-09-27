# Powershell Without Powershell.exe

Powershell.exe is just a process hosting the System.Management.Automation.dll which essentially the  Powershell as we know it.

If you run into a situation where powershell.exe is blocked and no strict application whitelisting is implemented, there are ways to execute powershell still.

## PowerShdll

```text
rundll32.exe PowerShdll.dll,main
```

![](../.gitbook/assets/pwshll-rundll32.gif)

Note that the same could be achieved with a compiled .exe binary from the same project, but keep in mind that .exe is more likely to run into whitelisting issues.

{% embed data="{\"url\":\"https://github.com/p3nt4/PowerShdll\",\"type\":\"link\",\"title\":\"p3nt4/PowerShdll\",\"description\":\"Run PowerShell with rundll32. Bypass software restrictions. - p3nt4/PowerShdll\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars0.githubusercontent.com/u/19682240?s=400&v=4\",\"width\":345,\"height\":345,\"aspectRatio\":1}}" %}
