# Understanding ICACLS permissions

From the Microsoft Article on ICACLS

The entries are users and groups specific to that file \(DOMAIN\USER or GROUP\), the permissions listed are as follows:

SIDs may be in either numerical or friendly name form. If you use a numerical form, affix the wildcard character \* to the beginning of the SID.

icacls preserves the canonical order of ACE entries as:

	• Explicit denials

	• Explicit grants

	• Inherited denials

	• Inherited grants

Perm is a permission mask that can be specified in one of the following forms:

	1. A sequence of simple rights:

		○ F \(full access\)

		○ M \(modify access\)

		○ RX \(read and execute access\)

		○ R \(read-only access\)

		○ W \(write-only access\)

	2. A comma-separated list in parenthesis of specific rights:

		○ D \(delete\)

		○ RC \(read control\)

		○ WDAC \(write DAC\)

		○ WO \(write owner\)

		○ S \(synchronize\)

		○ AS \(access system security\)

		○ MA \(maximum allowed\)

		○ GR \(generic read\)

		○ GW \(generic write\)

		○ GE \(generic execute\)

		○ GA \(generic all\)

		○ RD \(read data/list directory\)

		○ WD \(write data/add file\)

		○ AD \(append data/add subdirectory\)

		○ REA \(read extended attributes\)

		○ WEA \(write extended attributes\)

		○ X \(execute/traverse\)

		○ DC \(delete child\)

		○ RA \(read attributes\)

		○ WA \(write attributes\)

Inheritance rights may precede either Perm form, and they are applied only to directories:

	• \(OI\): object inherit

	• \(CI\): container inherit

	• \(IO\): inherit only

	• \(NP\): do not propagate inherit

	• \(I\): permission inherited from parent container



For files, the permission masks are more or less self-explanatory: R means you can read the file, Xallows it to be executed \(as a program\), and so on.

For other kinds of objects, you will have to browse MSDN:

	• Standard Access Rights

	• ACE Inheritance Rules

	• Registry

	• Service

	• ...

Inheritance rights in English:

	• \(I\) "Inherited": This ACE was inherited from the parent container.

	• \(OI\) "Object inherit": This ACE will be inherited by objects placed in this container.

	• \(CI\) "Container inherit": This ACE will be inherited by subcontainers placed in this container.

	• \(IO\) "Inherit only": This ACE will be inherited \(see OI and CI\), but does not apply to this object itself.

	• \(NP\) "Do not propagate": This ACE will be inherited by objects and subcontainers one level deep – it will not apply to things inside subcontainers.

For the file system, "container" means a folder and "object" means a file, but remember that ACLs can be set on many other kinds of objects, not all of which have a concept of "containers".



