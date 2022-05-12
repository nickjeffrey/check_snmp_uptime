# check_snmp_uptime
nagios check for uptime via SNMP for Windows, Linux, Cisco 

This script will check the uptime and alert on recent reboots

# Requirements
You will need to enable SNMP on the monitored device.  Windows / Linux / Cisco / etc are supported.  Other devices using same SNMP OID values work as well.

# Usage
Add the following section to services.cfg on the nagios server
```
# Check uptime via SNMP
# syntax is check_snmp_uptime!optional_snmp_community!optional_minutes_uptime_warn_threshold
define service{
        use                             generic-service
        hostgroup_name                  all_windows,all_linux,all_cisco
        service_description             uptime
        check_command                   check_snmp_uptime!optional_snmp_community!optional_minutes_uptime_warn_threshold
        }
```

Add the following section to commands.cfg on the nagios server
```
# ---------------------------------------------------------------------------
# 'check_snmp_uptime' command definition
define command{
        command_name    check_snmp_uptime
        command_line    $USER1$/check_snmp_uptime -H $HOSTADDRESS$ -C $ARG1$ -w $ARG2$
        }
```

Copy the `check_snmp_uptime` file to `/usr/local/nagios/libexec/` (or wherever your distro keeps nagios checks)

# Ouput
If the uptime is less than one day, the output will be shown in minutes.  For example:
```uptime OK - uptime is 122 minutes```
If the uptime is more than one day, the output will be shown in days.  For example:
```uptime OK - uptime is 17 days | uptime_days=2;;;;```
If the uptime is less than the threshold, an alert will be shown.  For example:
```uptime WARN - uptime is 3 minutes | uptime_days=0;;;```

