# Search for Files on Linux

## Filtering by Name

```bash
find -name <query>
```

### Case insensitive

```bash
find -iname <query>
```

### Exclude

```bash
find -not -name <query_to_avoid>
```

### Invert a query

```bash
find \! -name <query_to_avoid>
```

## Filtering by Type

```bash
find -type <type_descriptor> <query>
```

- `f`: regular file
- `d`: directory
- `l`: symbolic link
- `c`: character devices
- `b`: block devices

## Filtering by Size

```bash
find /usr -size <size>
```

- `c`: bytes
- `k`: kilobytes
- `M`: megabytes
- `G`: gigabytes
- `b`: 512-byte blocks

### Example: find files that are less than 50 bytes

```bash
find -size -50c
```

### Example: find files in the `/usr` directory that are more than 700 Megabytes

```bash
find /usr -size +700M
```

## Filtering by Time

For every file on the system, Linux stores time data about:

- **Access Time** (`-atime`): The last time a file was read or written to.

- **Modification Time** (`-mtime`): The last time the contents of the file were modified.

- **Change Time** (`-ctime`): The last time the file’s inode metadata was changed.

### Example: find files that were modified within the last day

```bash
find -mtime 1
```

### Example: findfiles that were accessed less than a day ago

```bash
find -atime -1
```

### Example: files that last had their meta information changed more than 3 days ago

```bash
find -ctime +3
```

## Finding by Owner and Permissions

```bash
find /var -user syslog

find /etc -group shadow

find / -perm 644
```

If you want to specify anything with at least those permissions:

```bash
find / -perm -644
```

## Filtering by Depth

```bash
find -maxdepth 2 -name file1

find -mindepth 4 -name file1

find -mindepth 2 -maxdepth 3 -name file1
```

## Executing Commands on `find` Results

```bash
find . -type f -perm 644 -exec chmod 664 {} \;
```

The `{}` is used as a placeholder for the files that find matches. The `\;` lets find know where the command ends.

## Finding Files Using `locate`

An alternative to using `find` is the `locate` command. This command is often **quicker** and can search the entire file system with ease.

```bash
sudo apt update
sudo apt install mlocate
```

The reason `locate` is faster than find is because it relies on a database that lists all the files on the filesystem. This database is usually updated once a day with a cron script, but you can update it manually with the updatedb command.

```bash
sudo updatedb
```

### Example: find any files and directories that contain the string query anywhere in their file path

```bash
locate <query>
```

### Example: find only for files whose “basename” matches the query

```bash
locate -b <query>
```
