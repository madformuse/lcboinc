FROM boinc/server_makeproject:4.0.1-b2d

# dont need built-in boinc2docker app
RUN rm -rf $PROJECT_ROOT/apps/boinc2docker

COPY --chown=1000 config.xml project.xml $PROJECT_ROOT/
COPY --chown=1000 project_files $PROJECT_ROOT