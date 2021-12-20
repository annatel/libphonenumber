VERSION 0.5

alpine-base:
    FROM alpine:3.12.1
    RUN apk add --no-progress --update git build-base
    RUN apk --no-cache --update add libgcc libstdc++ \
        git make g++ \
        build-base gtest gtest-dev boost boost-dev protobuf protobuf-dev cmake icu icu-dev openssl \
        && \
        rm -rf /var/cache/apk/*


libphonenumber:
    FROM +alpine-base

    WORKDIR /libphonenumber/cpp    
    COPY --dir cpp/src cpp/test .
    COPY cpp/CMakeLists.txt .

    WORKDIR /libphonenumber
    COPY --dir metadata resources tools .
    
    WORKDIR /libphonenumber/cpp/build 

    RUN cmake ..
    RUN make

    SAVE ARTIFACT /libphonenumber/cpp/build AS LOCAL cpp/

