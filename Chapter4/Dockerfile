FROM diamol/base AS build-stage
RUN echo 'building...' > /build.txt

FROM diamol/base AS test-stage
COPY --from=build-stage /build.txt /build.txt
RUN echo 'Testing..' >> /build.txt

FROM diamol/base
COPY --from=build-stage /build.txt /build.txt
CMD cat /build.txt