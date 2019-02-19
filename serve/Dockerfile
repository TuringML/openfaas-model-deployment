FROM tensorflow/serving

RUN mkdir -p /home/app

RUN apt-get update \
    && apt-get install curl -yy

RUN echo "Pulling watchdog binary from Github." \
    && curl -sSLf https://github.com/openfaas-incubator/of-watchdog/releases/download/0.4.6/of-watchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog

# Add non root user
# RUN addgroup app && adduser app -S -G app

# RUN chown app /home/app

WORKDIR /root/

# USER app

# remove unecessery logs from S3
ENV TF_CPP_MIN_LOG_LEVEL=3

ENV AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
ENV AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
ENV AWS_REGION=${AWS_REGION}
ENV S3_ENDPOINT=${S3_ENDPOINT} 

ENV fprocess="tensorflow_model_server --rest_api_port=8501 \
    --model_name=${MODEL_NAME} \
    --model_base_path=${MODEL_BASE_PATH}"

# Set to true to see request in function logs
ENV write_debug="true"
ENV cgi_headers="true"
ENV mode="http"
ENV upstream_url="http://127.0.0.1:8501"

# gRPC tensorflow serving
# EXPOSE 8500

# REST tensorflow serving
# EXPOSE 8501

RUN touch /tmp/.lock
HEALTHCHECK --interval=5s CMD [ -e /tmp/.lock ] || exit 1

CMD [ "fwatchdog" ]