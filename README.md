# K.I.T.Z.U.N.E
Kinetic Integrated Trustworthy Zone-based UNified Environment

# Aim
Distributed, Secured Filesystem using preexisting storage systems, without a central server.

# Core Components

```
┌──────────────────────────────────────────────────┐
│                  Unified Layer                   │
└──────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────┐
│                   Cache Layer                    │
└──────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────┐
│                    Rule Layer                    │
└──────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────┐
│                   Block Layer                    │
└──────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────┐
│                 Encryption Layer                 │
└──────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────┐
│┌─────────────────────────────────────────────────┴┐
└┤ ┌────────────────────────────────────────────────┴─┐
 └─┤           Endpoint Block Storage Layer           │
   └──────────────────────────────────────────────────┘
```

## Unified Layer
At this layer files are compared via a SHA256 hash, modified times and creation time. Files can be pushed and pulled in whole layers.

This layer will maintain a filesystem skeleton and load the actual files and resources when needed.
## Cache Layer
This layer will cache full decrypted versions of files in the cache directory or just a SHA256 hash, modified time and creation time.
## Rule Layer
Responsible for converting rules to the block layer commands. Since the computer can be shutdown at any time, it has the hardest job and must ensure that certain files and such are properly duplicated.
## Block Layer
Storing files in 15KiB sizes.
## Encyption Layer
1KiB encryption layer that encrypts using a rotation encryption scheme.
## Endpoint Block Storage Layer
Must support basic commands like time created (seconds since 1970 UTC), time modified (seconds since 1970 UTC), and checking for existence of certain UUID's

# Filesystem Blocks

There are four kinds of blocks:

## Genisis Block
This contains the version byte. 
Compacted service access information. 
    Note if there are more then 4 services, look at Service Spreading.
The filetype is a JSON file.
This contains UUID to root folder.
## File Block
- This contains a version byte.
- This contains a UUID to Folder Block. 
- This contains text (up to 256 UTF-4 characters)
- This contains text (up to 256 UTF-4 characters)
- Time created at (seconds since 1970)
- Time modified at (seconds since 1970)
- File storage priority (1 byte)
- Raw data
## Data Block
- This contains a version byte.
- This contains a UUID to File Block.
- Contains raw data
## Folder Block 
- This contains a version byte.
- This contains a UUID to parent Folder Block. -> Note that all folder blocks do not point to themselves except root folder (because there's no higher to go)
- This contains text (up to 256 UTF-4 characters)
- Time created at (seconds since 1970)
- Time modified at (seconds since 1970)
- List UUID's

## Extended Folder Block
- This contains a version byte.
- This contains a UUID to Folder Block.
- List of UUID's



# Terms

## Service Spreading

If there are more than 4 Endpoint Block Storage Layers used for storages then the first storage service has access to the first four, the second has access to itself up to service five and so on.

IE six Endpoint Block Storage Layers:

- 1234
- 2345
- 3456
- 4561
- 5612
- 6123

The point of servic spreading is the that only one service needs to be dropped into the K.I.T.Z.U.N.E. and it'll autodiscover the rest of the Endpoint Block Storage Layer(s).