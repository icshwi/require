
# ESS customized require originated from PSI

The following files are touched:

* require.c
* driver.makefile

And ESS doesn't use iocsh, we have the indepdent iocsh.bash, which is in https://github.com/icshwi/e3-require

```
require "<module>" , "<version>"
 ioc shell function
 Loads a module library and its dbd file (if not yet done).

The two arguments "<module>" and "<version>" are mandatory.
 "<version>" may be:
* full version number,    e.g. "1.2.3"   -- loads exact match
* test version,           e.g. "zimoch"  -- loads exact match

```

# Original README


```
require "<module>" [,"<version>"] [,"<macro1>=<value2>, <macro2>=<value2>"]
 ioc shell function
 Loads a module library and its dbd file (if not yet done).
 Executes its startup script if available.
 
 The two arguments "<version>" and the macro list are optional.
 The order does not matter, thus macros can be provided without a version.
 
 "<version>" may be:
    * full version number,    e.g. "1.2.3"   -- loads exact match
    * partial version number, e.g. "1.2"     -- loads highest available 1.2.x
    * minimum version number, e.g. "1.2+"    -- loads highest 1.x.y with x>=2
    * no version,             nothing or ""  -- loads highest available version
    * test version,           e.g. "zimoch"  -- loads exact match
    * optional flag           "ifexists"     -- loads highest version if one exists
    
 Failure to find a matching version aborts the startup script, except for "ifexists".
 
 If a version is already loaded, the requested version is checked for compatibility.
 If the versions are incompatible the startup script is aborted.
 An already loaded version is considered compatible if:
    * no specific version was required
    * the loaded version matches the required version exactly
    * major numbers are the same and loaded minor number is at least the required one
    * major and minor numbers are the same and patch number is at least the required one
    * the loaded version is a test version and the required version is not a test version
 
 Versions with different major numbers are never compatible.
 Different test versions are never compatible.
 
 If available, a module startup script is executed. Macros of the form $(macro) are
 replaced either with given arguments or with environment variables in that script.
 
 The first found script in the module directory of the following list is executed:
   * <targetArch>.cmd  e.g. T2-ppc604.cmd
   * <osClass>.cmd     e.g. Linux.cmd
   * startup.cmd
 
 If the module was already loaded, the startup script is called again only if
 macro parameters are given.
 
 The following environment variables are set:
   * MODULE                 the name of the current module
   * T_A                    the target architecture, e.g "T2-ppc604"
   * EPICS_RELEASE          the EPICS base release, e.g. "3.14.12"
   * <module>_DIR           the directory of the loaded version of the module
   * TEMPLATES              the db directory of the module
   * <module>_TEMPLATES     the db directory of the module
   * EPICS_DB_INCLUDE_PATH  the db directories of all modules, last reqired first (after ".")

   The variables MODULE and TEMPLATES get overwritten for each module.
   They always refer to the last require call, even if the module was already loaded.
   The variable EPICS_DB_INCLUDE_PATH is used by dbLoadRecords and dbLoadDatabase.
   ```
 
