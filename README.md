# Apollo S3DataSource

[![CircleCI](https://circleci.com/gh/ovotech/apollo-datasource-s3.svg?style=svg&circle-token=c64973d3fd46a2b132c93516f9f44df992380a3b)](https://circleci.com/gh/ovotech/apollo-datasource-s3)
[![npm (scoped)](https://img.shields.io/npm/v/@ovotech/apollo-datasource-s3.svg)](https://www.npmjs.com/package/@ovotech/apollo-datasource-s3)

S3DataSource is responsible for fetching data from a given S3 bucket using aws js API. Integrates with the cache and expires tags on S3, following the example of [Apollo Data Sources](https://www.apollographql.com/docs/apollo-server/features/data-sources.html). This useful if you want to read data from a private / restricted s3 bucket.

### Using

```bash
yarn add @ovotech/apollo-datasource-s3
```

This module ships with TypeScript types. It also has helper methods to deal with JSON objects directly.

```ts
import { S3DataSource } from '@ovotech/apollo-datasource-s3';
import { S3 } from 'aws-sdk';

class MyS3DataSource extends S3DataSource {
  async get() {
    return await this.getObjectJson({ Bucket: 'bucket', Key: 'data.json' });
  }

  async put(data) {
    return await this.putObjectJson(data, { Bucket: 'bucket', Key: 'test' });
  }
}

const s3 = new S3();
const ds = new MyS3DataSource(s3);
```

You can also access the lower level api to load the raw data from s3, as well as list and delete items. Those request mimic the s3 api itself, but adds a layer of caching in front of it.

```ts
class MyS3DataSource extends S3DataSource {
  async get() {
    return await this.getObject({ Bucket: 'bucket', Key: 'data.json' });
  }

  async put(data) {
    return await this.putObject({ Bucket: 'bucket', Key: 'test', Body: 'data' });
  }

  async list() {
    return await this.listObjects({ Bucket: 'bucket' });
  }

  async delete() {
    return await this.deleteObject({ Bucket: 'bucket', Key: 'data.json' });
  }
}
```

## Running the tests

The tests require a running s3 mock server, and we're using [localstack](https://github.com/localstack/localstack) for that.
You'll need to start the s3 server:

```bash
SERVICES=s3 localstack start
```

After which you can run all the tests:

```bash
yarn test
```

### Coding style (linting, etc) tests

Style is maintained with prettier and tslint

```
yarn lint
```

## Deployment

To deploy a new version, push to master and then create a new release. CircleCI will automatically build and deploy a the version to the npm registry.

## Contributing

Have a bug? File an issue with a simple example that reproduces this so we can take a look & confirm.

Want to make a change? Submit a PR, explain why it's useful, and make sure you've updated the docs (this file) and the tests (see `test/S3DataSource.spec.ts`). You can run the tests with `SERVICES=s3 localstack start` and `yarn test`.

## Responsible Team

- Boost Internal Tools (BIT)

## License

This project is licensed under Apache 2 - see the [LICENSE](LICENSE) file for details
