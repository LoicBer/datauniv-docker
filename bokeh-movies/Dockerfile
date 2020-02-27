# 1 use official python3.7 image from Dockerhub as base image
FROM python:3.7

# 2 create bokeh user to avoid running as root user
RUN useradd --create-home bokeh

# 3 create /app directory
RUN mkdir /app

# 4 copy source directory to /app/movies
ADD src/ /app/movies

# 5 transfer ownership of /app/movies/ directory to bokeh user
# Hint: chown -R bokeh /app/movies
RUN chown -R bokeh /app/movies/

# 6 install python dependencies (bokeh and pandas) of the app
# Hint: pip install ...
RUN pip install bokeh pandas

# 7 change working directory to /app/movies
WORKDIR /app/movies

# 8 switch to bokeh user
USER bokeh

# 9 download sample data
RUN python -c "import bokeh.sampledata; bokeh.sampledata.download()"
# It's dirty because no data should be stored in the image. Use volume at runtime instead.

# 10 declare exposed port
EXPOSE 5006

# 11 set entrypoint to start bokeh server
ENTRYPOINT bokeh serve --show --allow-websocket-origin 127.0.0.1:8080 .

