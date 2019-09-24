# 1 use official python3.7 image from Dockerhub as base image
FROM python:3.7

# 2 create bokeh user to avoid running as root user

# 3 create /app directory

# 4 copy source directory to /app/movies

# 5 transfer ownership of /app/movies/ directory to bokeh user

# 6 install python dependencies (bokeh and pandas) of the app

# 7 change working directory to /app/movies

# 8 switch to bokeh user

# 9 download sample data
RUN python -c "import bokeh.sampledata; bokeh.sampledata.download()"

# 10 declare exposed port

# 11 set entrypoint to start bokeh server

