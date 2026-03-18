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
- Location: NAS storage device
- Capacity: 3 TB available storage
- Connection: Network attached

### Secondary Storage
- Location: Off-site backup location
- Capacity: 4 TB available storage
- Frequency: Monthly transfers

## Retention Policy

| Backup Type | Retention Period | Storage Location |
|-------------|------------------|------------------|
| Daily | 7 days | Primary |
| Weekly | 4 weeks | Primary + Secondary |
| Monthly | 12 months | Secondary |

## Recovery Procedures

- **Point-in-Time Recovery**: Steps to restore data from a specific backup TBA
- **Full System Recovery**: Steps to restore entire infrastructure TBA
- **Selective Recovery**: Steps to restore specific files or VMs TBA

## Testing and Validation

- Backup integrity checks: Weekly
- Recovery drills: Quarterly
- Documentation review: Monthly
