#  TrueNAS

## General
----
We have a NAS unit running [TrueNAS](https://www.truenas.com/) at [vault.synshop.net](http://vault.synshop.net) which is only accessible on the shop LAN and not accessible on the internet. Any member who requires some storage space for project related storage can request an account to be created for them.

- There is also a folder called "stash" that is a public, general temporary space that is mounted as a shared storage by all shop computers to quickly and easily move files between devices.
- The Vault pool has a folder called "members" under which each user with an active account on the NAS has a personal folder.
- The stash folder is at the same level.
- Additional folders can be created as needed.
- There are two user groups: Members and Admins. Folders can be given Members, Admin, or selected individual permissions as needed. More groups can be created if required but is not advised.

## Administration
----
To add a new user to the system, you need to log into the [web interface](http://vault.synshop.net) (or alternatively http://10.0.40.30) as the `root` user. Credentials for `root` in the administrative keepass in Google Drive under `TrueNAS`. (*Your individual user creds WILL NOT work!*)
To add a user:

1. Go to Accounts>User in the left hand menu
2. Click "Add User"
3. Fill in the actual members name, and their chosen username (default to their Discord/known-as identity when possible)
4. The email address is optional, but preferred. Use the member's normal contact email for future compatibility.
5. Set a password for the member, or let them set one themselves. Note that they can only change it by SSHing into the NAS.
6. Leave the userID to the programatically assigned one.
7. Uncheck "new primary group" and set their group to the existing "members" group.
8. select /mnt/vault/members from the menu as their default home directory location. (the system will auto create a folder for them with their username)
9. Submit!


## Hardware
----
The unit is a Rackable Systems with 1x2TB and 3x12TB SATA drives, with a SAS cable to a 16 drive unit with a mix of 2tb and 3tb drives.

- Drive 0 (far left) in the control unit is the OS drive at 2TB
- Drives 1-3 in the control unit are 12TB drives in a RAIDz1 pool called "Plex" for media storage managed by Aakin
- Drive 4 is unused (due to a clerical error thanks so a bay tracking/sn logging mixup)
- Drives 5-7 are 2TB drives in a RAIDz1 pool called "Jail" for the plugin system
- Drives 8-19 are 3Tb drives in a RAIDz3 pool called "Vault", the main NAS storage.

Actual drive tracking is done via description notes in the Storage panel of TrueNAS. Each drive's "description" identifies what bay it is located in, as the daX numbering does not match the bays, even sequentially.

There are two network devices. LAN 1 is static assigned to 10.0.40.30. TrueNAS takes a hardline philosphical stance on never having 2 NICs on the same subnet, and has designed many elements of the OS to prevent this, so LAN 2 is not used at this time until we have a LAG capable switch to hardware LAG them (which *is* supported).

Future to do:

- Setup second nic on the local network on an appropriately different subnet or LAG
- Bind both nics into one for higher throughput and reliability
- Eventually replace user management with centralized system for the whole shop
- Replace the Vault array 3tb drives with larger capacity (and newer!) ones and resilver+expand the pool.