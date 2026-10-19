FROM spark:4.1.3-scala2.13-java17-python3-ubuntu
USER root
# Iceberg installé dans Spark lui-même (et non téléchargé au démarrage)
ADD https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-4.1_2.13/1.12.0/iceberg-spark-runtime-4.1_2.13-1.12.0.jar /opt/spark/jars/
ADD https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-aws-bundle/1.12.0/iceberg-aws-bundle-1.12.0.jar /opt/spark/jars/
RUN chmod 644 /opt/spark/jars/iceberg-*.jar
USER spark