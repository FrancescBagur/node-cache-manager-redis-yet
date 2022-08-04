# Redis store for node cache manager

Redis cache store for [node-cache-manager](https://github.com/BryanDonovan/node-cache-manager).

## Installation

```sh
pnpm install cache-manager-redis-yet
```

## NestJs

#### Module

```typescript
import { CacheModule } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { redisStore } from 'cache-manager-redis-yet';

@Module({
  imports: [
    CacheModule.registerAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: async (config: ConfigService) => {
        const conf = {
          url: config.get<string>('REDIS_URL'),
          ttl: config.get<number>('REDIS_CACHE_TTL'),
        };
        return {
          store: await redisStore(conf),
          ...conf,
        };
      },
    }),
  ],
})
export class CacheModule {}
```

### NestJs Service

```typescript
import { CACHE_MANAGER, Inject, Injectable } from '@nestjs/common';
import { RedisCache } from 'cache-manager-redis-yet';

@Injectable()
export class CacheService {
  constructor(@Inject(CACHE_MANAGER) private readonly cache: RedisCache) {}
}
```

## License

Licensed under the [MIT license](./LICENSE).

Fork from [https://github.com/dabroek/node-cache-manager-redis-store](https://github.com/dabroek/node-cache-manager-redis-store)
