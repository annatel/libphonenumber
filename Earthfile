VERSION 0.5

alpine-base:
    FROM alpine:3.12.1
    RUN apk add --no-progress --update git build-base zip
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

    WORKDIR /libphonenumber/cpp
    RUN mkdir -p assets/build
    RUN find ./build -type f -name "libphonenumber*" -exec cp {} assets/build \;
    RUN find ./build -type f -name "libgeocoding*" -exec cp {} assets/build \;
    RUN mkdir -p assets/includes/phonenumbers/{base,geocoding,utf}
    RUN find ./src/phonenumbers  -maxdepth 1 -type f -name "*.h" -exec cp {} assets/includes/phonenumbers \;
    RUN find ./src/phonenumbers/base -maxdepth 1 -type f -name "*.h" -exec cp {} assets/includes/phonenumbers/base \;
    RUN find ./src/phonenumbers/geocoding -maxdepth 1 -type f -name "*.h" -exec cp {} assets/includes/phonenumbers/geocoding \;
    RUN find ./src/phonenumbers/utf -maxdepth 1 -type f -name "*.h" -exec cp {} assets/includes/phonenumbers/utf \;
    RUN zip -r assets.zip assets/

    SAVE ARTIFACT /libphonenumber/cpp/assets.zip AS LOCAL cpp/assets.zip

