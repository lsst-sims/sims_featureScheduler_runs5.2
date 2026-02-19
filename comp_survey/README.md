To avoid confusion, we have "comp_survey" rather than "baseline".


Pulling setup from ts_config_scheduler:

cp ~/git_repos/ts_config_scheduler/Scheduler/feature_scheduler/maintel/fbs_config_lsst_survey_block_419.py .
cp ~/git_repos/ts_fbs_utils/python/lsst/ts/fbs/utils/maintel/lsst_surveys.py .
cp ~/git_repos/ts_fbs_utils/python/lsst/ts/fbs/utils/maintel/roman_surveys.py .
cp ~/git_repos/ts_config_scheduler/Scheduler/ddf_gen/lsst_ddf_gen_block_419.py .
cp ~/git_repos/ts_config_scheduler/Scheduler/ddf_gen/gen_ddf_presched_observations.py .

cp ~/git_repos/ts_fbs_utils/python/lsst/ts/fbs/utils/maintel/too_surveys.py .

Updates to code that might need to get merged back in:

* imports for lsst_survey, roman_survey, too_survey
* swap of SURVEY_START_MJD
* add generate_qm
* ran new ddf npz and updated path to it



Changes from v5.2

* updating survey start date to april 1, 2025
