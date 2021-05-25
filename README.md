# im3-clm
This is the IM3 fork of CTSM, which maintains support for IM3 machines (cori and constance) as well as adds certains features to CLM which are under active development by NCAR: PPE (parameter tuning), and HH (hillslope hydrology).

## quickstart
For the full documentation, please see the original CTSM repository: https://github.com/ESCOMP/CTSM.

* Clone the repository: `git clone https://github.com/IMMM-SFA/im3-clm.git`
* Change to the repository directory: `cd im3-clm`
* Install dependencies: `./manage_externals/checkout_externals`
* Create a new case: `./cime/scripts/create_newcase --case ~/cases/clm5-sp --res CLM_USRDAT --mach cori-knl --compset I2000Clm50Sp --project m2702 --run-unsupported`
  * replace the case directory `~/cases/clm5-sp` with a path and name of your choice
  * replace the machine based on where you are running, i.e. `cori-knl`, `cori-haswell`, or `constance`
  * replace the compset with your desired compset, typical options are `I2000Clm50Sp` for Satellite-Phrenology mode or `I2000Clm50Bgc` for Biogeochem mode (see `./cime_config/config_compsets.xml` for all available compsets`)
  * replace the project code based on the machine you are on: `m2702` for cori or `IM3` for constance
* Change to the case directory: `cd ~/cases/clm5-sp` (replace with your chosen case directory)
* Setup the case: `./case.setup`
* Update the following settings to your desired values (this example is for a 1/8 degree CONUS scale 16pft SP simulation):
  * `./xmlchange JOB_WALLCLOCK_TIME=10:00:00`
  * `./xmlchange NTASKS_ATM=256`
  * `./xmlchange NTASKS_LND=256`
  * `./xmlchange NTASKS_CPL=256`
  * `./xmlchange RUN_STARTDATE=2001-01-01`
  * `./xmlchange CLM_USRDAT_NAME=NLDAS2`
  * `./xmlchange LND_DOMAIN_FILE=domain.lnd.nldas2_16pfts_simyr2000.nc`
  * `./xmlchange ATM_DOMAIN_FILE=domain.lnd.nldas2_16pfts_simyr2000.nc`
  * `./xmlchange ATM_DOMAIN_PATH="\$DIN_LOC_ROOT/share/domains"`
  * `./xmlchange LND_DOMAIN_PATH="\$DIN_LOC_ROOT/share/domains"`
  * `./xmlchange STOP_N=10`
  * `./xmlchange REST_N=1`
  * `./xmlchange STOP_OPTION=nyears`
  * `./xmlchange REST_OPTION=nyears`
  * `./xmlchange DATM_CLMNCEP_YR_START=2001`
  * `./xmlchange DATM_CLMNCEP_YR_END=2011`
  * `./xmlchange DATM_CLMNCEP_YR_ALIGN=2001`
  * `./xmlchange DIN_LOC_ROOT=/project/projectdirs/m2702/hongxiang/inputdata`
  * `./xmlchange DIN_LOC_ROOT_CLMFORC="\$DIN_LOC_ROOT/atm/datm7"`
  * `./xmlchange DOUT_S_ROOT=clm_archive`
  * `./xmlchange DATM_MODE=CLM1PT`
  * `./xmlchange DOUT_S_SAVE_INTERIM_RESTART_FILES=TRUE`
* Update or add the following settings to the `user_nl_clm` file based on your simulation:
  * `fsurdat = '/project/projectdirs/m2702/hongxiang/inputdata/lnd/clm2/surfdata_map/surfdata_nldas2_16pfts_simyr2000.nc'`
  * `hist_nhtfrq = -24`
  * `hist_mfilt  = 30`
  * `finidata = '<path to a previous restart file, to set initial conditions, if desired>'`
* Create a file `user_datm.streams.txt.CLM1PT.CLM_USRDAT` that has your climate forcing data. It should look something like this:
```xml
<?xml version="1.0"?>
<file id="stream" version="1.0">
<dataSource>
   GENERIC
</dataSource>
<domainInfo>
  <variableNames>
     time    time
     xc      lon
     yc      lat
     area    area
     mask    mask
  </variableNames>
  <filePath>
     /project/projectdirs/m2702/hongxiang/inputdata/share/domains
  </filePath>
  <fileNames>
     domain.lnd.nldas2_16pfts_simyr2000.nc
  </fileNames>
</domainInfo>
<fieldInfo>
   <variableNames>
        TBOT     tbot
	QBOT     shum
        WIND     wind
        PRECTmms precn
        FSDS     swdn
        PSRF     pbot
        FLDS     lwdn
   </variableNames>
   <filePath>
     /project/projectdirs/m2702/hongxiang/inputdata/atm/datm7/NLDAS2/CLM1PT_data
   </filePath>
   <fileNames>
     2001-01.nc
     2001-02.nc
     2001-03.nc
     2001-04.nc
     2001-05.nc
     2001-06.nc
     2001-07.nc
     2001-08.nc
     2001-09.nc
     2001-10.nc
     2001-11.nc
     2001-12.nc
     2002-01.nc
     2002-02.nc
     2002-03.nc
     2002-04.nc
     2002-05.nc
     2002-06.nc
     2002-07.nc
     2002-08.nc
     2002-09.nc
     2002-10.nc
     2002-11.nc
     2002-12.nc
     2003-01.nc
     2003-02.nc
     2003-03.nc
     2003-04.nc
     2003-05.nc
     2003-06.nc
     2003-07.nc
     2003-08.nc
     2003-09.nc
     2003-10.nc
     2003-11.nc
     2003-12.nc
     2004-01.nc
     2004-02.nc
     2004-03.nc
     2004-04.nc
     2004-05.nc
     2004-06.nc
     2004-07.nc
     2004-08.nc
     2004-09.nc
     2004-10.nc
     2004-11.nc
     2004-12.nc
     2005-01.nc
     2005-02.nc
     2005-03.nc
     2005-04.nc
     2005-05.nc
     2005-06.nc
     2005-07.nc
     2005-08.nc
     2005-09.nc
     2005-10.nc
     2005-11.nc
     2005-12.nc
     2006-01.nc
     2006-02.nc
     2006-03.nc
     2006-04.nc
     2006-05.nc
     2006-06.nc
     2006-07.nc
     2006-08.nc
     2006-09.nc
     2006-10.nc
     2006-11.nc
     2006-12.nc
     2007-01.nc
     2007-02.nc
     2007-03.nc
     2007-04.nc
     2007-05.nc
     2007-06.nc
     2007-07.nc
     2007-08.nc
     2007-09.nc
     2007-10.nc
     2007-11.nc
     2007-12.nc
     2008-01.nc
     2008-02.nc
     2008-03.nc
     2008-04.nc
     2008-05.nc
     2008-06.nc
     2008-07.nc
     2008-08.nc
     2008-09.nc
     2008-10.nc
     2008-11.nc
     2008-12.nc
     2009-01.nc
     2009-02.nc
     2009-03.nc
     2009-04.nc
     2009-05.nc
     2009-06.nc
     2009-07.nc
     2009-08.nc
     2009-09.nc
     2009-10.nc
     2009-11.nc
     2009-12.nc
     2010-01.nc
     2010-02.nc
     2010-03.nc
     2010-04.nc
     2010-05.nc
     2010-06.nc
     2010-07.nc
     2010-08.nc
     2010-09.nc
     2010-10.nc
     2010-11.nc
     2010-12.nc
     2011-01.nc
     2011-02.nc
     2011-03.nc
     2011-04.nc
     2011-05.nc
     2011-06.nc
     2011-07.nc
     2011-08.nc
     2011-09.nc
     2011-10.nc
     2011-11.nc
     2011-12.nc
        </fileNames>
   <offset>
      0
   </offset>
</fieldInfo>
</file>
```
* Build your case: `./case.build`
* Submit your simulation to the queue: `./case.submit`

