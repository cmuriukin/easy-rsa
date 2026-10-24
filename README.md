[![CI](https://github.com/Openvpn/easy-rsa/actions/workflows/action.yml/badge.svg)](https://github.com/Openvpn/easy-rsa/actions/workflows/action.yml)
# Overview

easy-rsa is a CLI utility to build and manage a PKI CA. In layman's terms,
this means to create a root certificate authority, and request and sign
certificates, including intermediate CAs and certificate revocation lists (CRL).

# Downloads

If you are looking for release downloads, please see the releases section on
GitHub. Releases are also available as source checkouts using named tags.

# Documentation

For 3.x project documentation and usage, see the [README.quickstart.md](README.quickstart.md) file or
the more detailed docs under the [doc/](doc/) directory. The .md files are in Markdown
format and can be converted to html files as desired for release packages, or
read as-is in plaintext.

# Getting help using easy-rsa

Currently, Easy-RSA development co-exists with OpenVPN even though they are
separate projects. The following resources are good places as of this writing to
seek help using Easy-RSA:

The [openvpn-users mailing list](https://lists.sourceforge.net/lists/listinfo/openvpn-users)
is a good place to post usage or help questions.

Users of GitHub can report problems to the Easy-RSA `issues` list.

# Branch structure

The easy-rsa master branch is currently tracking development for the 3.x release
cycle. Please note that, at any given time, master may be broken. Feel free to
create issues against master, but have patience when using the master branch. It
is recommended to use a release, and priority will be given to bugs identified in
the most recent release.

The prior 2.x and 1.x versions are available as release branches for
tracking and possible back-porting of relevant fixes.

Branch layout is:

    master             <- Active: v3.2.x - Rolling.
    v3.<N>.<N>-<LABEL>    Active: Development branches.
    testing               Sandbox: Subject to change without notice.
    v3.1.8                Sunset: Bugfix only for v3.1.7

    The following are NOT compatible with OpenSSL version 3:

    v3.0.6                Inactive: Archived.
    v3.0.5                Inactive: Archived.
    v3.0.4                Inactive: Archived.
    release/3.0           Inactive: Archived.
    release/2.x           Inactive: Archived.
    release/1.x           Inactive: Unmaintained.

LICENSING info for 3.x is in the [COPYING.md](COPYING.md) file

## Contributing

Please refer to: [doc/EasyRSA-Contributing.md](doc/EasyRSA-Contributing.md)

# Code style, standards

We are attempting to adhere to the POSIX standard, which can be found here:

https://pubs.opengroup.org/onlinepubs/9699919799/


######################################################################

Step 1: Generate Certificates:

sudo yum -y install git
git clone https://github.com/OpenVPN/easy-rsa.git
cd easy-rsa/easyrsa3
./easyrsa init-pki
./easyrsa build-ca nopass
./easyrsa build-server-full server nopass
./easyrsa build-client-full client1.net.internal nopass


Step 2: Copy Certificates to Central Folder:


mkdir ~/custom_folder/
cp pki/ca.crt ~/custom_folder/
cp pki/issued/server.crt ~/custom_folder/
cp pki/private/server.key ~/custom_folder/
cp pki/issued/client1.kplabs.internal.crt ~/custom_folder
cp pki/private/client1.kplabs.internal.key ~/custom_folder/
cd ~/custom_folder/


Step 3: Upload Certificate to ACM:



aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt --region ap-southeast-1



Step 4: Copy the Certificates to Laptop:

scp ec2-user@IP:/home/ec2-user/custom_folder/client* .



Step 5: Central Client Configuration File:

Following contents to be added to central ovpn configuration file -

<cert>
Contents of client certificate (.crt) file
</cert>


<key>
Contents of private key (.key) file
</key>


Step 6: Prepend the DNS Name.


