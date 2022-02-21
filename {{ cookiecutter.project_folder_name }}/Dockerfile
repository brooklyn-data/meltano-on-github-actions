FROM meltano/meltano:v1.96.0-python3.8

# Meltano project and install
COPY . /project/
RUN meltano install

# GitHub actions sets the working directory to the repo root when running.
# The entrypoint script sets the current directory to the meltano project and
# runs the container arguments as a command.
ENTRYPOINT ["./entrypoint.sh"]
