# Investigation of Issue #234 - mdLifetime Type Analysis

## Issue Description
Issue #234 suggested that there was a type mismatch between `mdLifetime` in `SaslOauthbearerToken` and the parameter expected by `rd_kafka_oauthbearer_set_token`.

## Investigation Findings

### Current Implementation
1. In `include/kafka/ClientCommon.h`, `mdLifetime` is correctly defined as milliseconds:
```cpp
struct SaslOauthbearerToken
{
    // ...
    std::chrono::milliseconds mdLifetime{};
    // ...
};
```

2. In `include/kafka/KafkaClient.h`, the value is correctly passed:
```cpp
rd_kafka_oauthbearer_set_token(rk,
                             oauthbearerToken.value.c_str(),
                             oauthbearerToken.mdLifetime.count(),
                             oauthbearerToken.mdPrincipalName.c_str(),
                             // ...
```

### Conclusion
No code changes are needed. The implementation already correctly uses milliseconds throughout:
- `std::chrono::milliseconds` for storage
- `.count()` to get the milliseconds value when passing to the C API
