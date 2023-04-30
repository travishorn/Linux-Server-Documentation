TODO: Finish this guide

# Linux Server

## Restoring Database Backups

### Prerequisites

- 

### Restore

```sh
sudo mkdir mybackup
sudo gunzip -c backup_2023-04-30T03\:14\:58Z.gz | sudo mbstream -x --directory=./mybackup
```
