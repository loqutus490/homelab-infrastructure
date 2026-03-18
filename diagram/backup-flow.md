# Backup Flow

This document describes the backup strategy and flow for the homelab infrastructure, including backup schedules, retention policies, and recovery procedures.

## Backup Strategy

### Backup Types
- **Full Backup**: Complete snapshot of all data
- **Incremental Backup**: Changes since the last backup
- **Differential Backup**: Changes since the last full backup

### Backup Schedule
- Full backups: Weekly (Sunday at 2:00 AM)
- Incremental backups: Daily (2:00 AM)
- Differential backups: On-demand

## Backup Sources

- **VMs**: All virtual machines
- **Containers**: Container volumes and persistent data
- **Configuration Files**: System and application configs
- **Databases**: Application databases

## Backup Destinations

### Primary Storage
- Location: NAS or external storage device
- Capacity: Specify available storage
- Connection: Network or direct attached

### Secondary Storage
- Location: Off-site backup location
- Capacity: Specify available storage
- Frequency: Weekly or monthly transfers

## Retention Policy

| Backup Type | Retention Period | Storage Location |
|-------------|------------------|------------------|
| Daily | 7 days | Primary |
| Weekly | 4 weeks | Primary + Secondary |
| Monthly | 12 months | Secondary |

## Recovery Procedures

- **Point-in-Time Recovery**: Steps to restore data from a specific backup
- **Full System Recovery**: Steps to restore entire infrastructure
- **Selective Recovery**: Steps to restore specific files or VMs

## Testing and Validation

- Backup integrity checks: Weekly
- Recovery drills: Quarterly
- Documentation review: Monthly
