VERSION 0.5

alpine-base:
    FROM alpine:3.12.1
    WORKDIR /app
    RUN apk add --no-progress --update git build-base
    RUN apk --no-cache --update add libgcc libstdc++ \
        git make g++ \
        build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
        && \
        rm -rf /var/cache/apk/*


libphonenumber:
    FROM +alpine-base
    WORKDIR /google
    GIT CLONE --branch antl https://github.com/annatel/libphonenumber.git libphonenumber
    WORKDIR /google/libphonenumber
    
    WORKDIR /google/libphonenumber/cpp/build 
    RUN cmake ..
    RUN make

    SAVE ARTIFACT /google/libphonenumber/cpp/build

docker:
    FROM alpine:3.12.1

    COPY --dir +libphonenumber/build ./libphonenumber/build
    WORKDIR /libphonenumber
    ARG EARTHLY_GIT_PROJECT_NAME=annatel/libphonenumber
    ARG TAG
  
    SAVE IMAGE --push ghcr.io/$EARTHLY_GIT_PROJECT_NAME:$TAG



