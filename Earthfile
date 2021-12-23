VERSION 0.5

alpine-base:
    FROM elixir:1.12.2-alpine
    RUN apk add --no-progress --update git build-base zip
    RUN apk --no-cache --update add libgcc libstdc++ \
        git make g++ \
        build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
        && \
        rm -rf /var/cache/apk/*
    RUN apk add openjdk8-jre

libphonenumber:
    FROM +alpine-base

    WORKDIR /libphonenumber
    COPY --dir . .

    # kludge: COPY creates files in the past causing make not rebuild some resources
    RUN find . -print0 | xargs -0 touch
    
    WORKDIR /libphonenumber/cpp/build
    RUN mkdir ./assets
    RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=./assets -DREGENERATE_METADATA=ON ..
    RUN make install

    WORKDIR assets
    RUN zip -r ../assets.zip *
    
    SAVE ARTIFACT /libphonenumber/cpp/build/assets.zip AS LOCAL cpp/build/assets.zip