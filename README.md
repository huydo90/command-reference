# COMMAND REFERENCE
List of operation commands

## Change Java default version
```
alternatives --config java
```

## Check Linux params
```bash
#!/bin/bash

# 0. Environment Detection
if [ -f /.dockerenv ] || grep -q 'docker' /proc/1/cgroup 2>/dev/null; then
    echo "Environment: Docker Container"
    # Cgroup memory limit check
    [ -f /sys/fs/cgroup/memory/memory.limit_in_bytes ] && echo "Container Memory Limit (v1): $(cat /sys/fs/cgroup/memory/memory.limit_in_bytes)"
    [ -f /sys/fs/cgroup/memory.max ] && echo "Container Memory Limit (v2): $(cat /sys/fs/cgroup/memory.max)"
else
    echo "Environment: Bare Metal / VM"
fi

# 1. Process & System Resource Limits (ulimit)
echo "Max Open Files (soft): $(ulimit -Sn)"
echo "Max Open Files (hard): $(ulimit -Hn)"
echo "Max User Processes (soft): $(ulimit -Su)"
echo "Max User Processes (hard): $(ulimit -Hu)"
echo "Core Dump File Size: $(ulimit -c)"

# 2. Virtual Memory Parameters
[ -f /proc/sys/vm/max_map_count ] && echo "Virtual Memory Max Map Count: $(sysctl -n vm.max_map_count)" || echo "Virtual Memory Max Map Count: Restricted/Unavailable"
[ -f /proc/sys/vm/overcommit_memory ] && echo "Virtual Memory Overcommit Memory: $(sysctl -n vm.overcommit_memory)"
[ -f /proc/sys/vm/swappiness ] && echo "Virtual Memory Swappiness: $(sysctl -n vm.swappiness)"

# 3. Networking Limits
[ -f /proc/sys/net/core/somaxconn ] && echo "Network Max Socket Connections: $(sysctl -n net.core.somaxconn)" || echo "Network Max Socket Connections: Restricted/Unavailable"
[ -f /proc/sys/net/ipv4/ip_local_port_range ] && echo "Network Local Port Range: $(sysctl -n net.ipv4.ip_local_port_range | tr -s ' ' '\t')"
[ -f /proc/sys/net/ipv4/tcp_tw_reuse ] && echo "Network TCP TIME_WAIT Reuse: $(sysctl -n net.ipv4.tcp_tw_reuse)"
[ -f /proc/sys/net/ipv4/tcp_fin_timeout ] && echo "Network TCP FIN Timeout: $(sysctl -n net.ipv4.tcp_fin_timeout)"

# 4. Filesystem Watchers
[ -f /proc/sys/fs/inotify/max_user_watches ] && echo "Filesystem Max User Inotify Watches: $(sysctl -n fs.inotify.max_user_watches)"

```
