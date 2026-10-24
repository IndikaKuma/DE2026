# ==========================================
# Stage 1: Grab the specific Python version
# ==========================================
FROM python:3.13.17 AS python-source

# ==========================================
# Stage 2: Final Spark image
# ==========================================
FROM apache/spark:4.2.0 AS final

# switch to root user
USER root

# Copy the Python installation from the first stage
COPY --from=python-source /usr/local /usr/local

# CRITICAL: Configure Spark to use the newly injected Python path
ENV PYSPARK_PYTHON=/usr/local/bin/python3
ENV PYSPARK_DRIVER_PYTHON=/usr/local/bin/python3

# Copy requirements.txt  to the working directory
COPY requirements.txt .
# Install required python packages
RUN  apt-get update -y && apt-get install -y python3-wheel
RUN python3.13 -m pip install --no-cache-dir -r requirements.txt

# Update the Guava library 
RUN echo 'spark.jars.packages com.google.guava:failureaccess:1.0.3,com.google.guava:guava:33.7.2-jre >> "${SPARK_HOME}/conf/spark-defaults.conf"

# Verify the replacement during build
RUN python3 --version && pyspark --version

USER spark