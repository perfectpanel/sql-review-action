## Изменено в Fork-е

- [Разрешен анализ всех типов файлов](https://github.com/perfectpanel/sql-review-action/blob/main/main.sh#L54) (по умолчанию библиотека дает валидировать только .sql файлы)
- [Запрещен выброс warning-ов и всех ошибок, кроме 408 (комментарии к столбцам](https://github.com/perfectpanel/sql-review-action/blob/main/sql-review.sh#L110)



# SQL Review Action

The GitHub Action for SQL Review. Parse and check the SQL statement according to the [SQL review rules](https://www.bytebase.com/sql-review-guide) to detect SQL anti-patterns and enforce schema consistency across the organization.

## Usage

Create a file in `.github/workflows/sql-review.yml` in your repository and insert the following content:

```yml
on: [pull_request]

jobs:
  sql-review:
    runs-on: ubuntu-latest
    name: SQL Review
    steps:
      - uses: actions/checkout@v3
      - name: Check SQL
        # You can change it to a specific version like bytebase/sql-review-action@0.0.4
        # We suggest using the latest version through the tag. Check it at https://github.com/Bytebase/sql-review-action/tags
        uses: bytebase/sql-review-action@main
        with:
          override-file-path: "<Your SQL review rules configuration file path>"
          template-id: "<SQL review rule template id>"
          database-type: "<The database type>"
          file-pattern: "<The file pattern for your SQL files>"
```

The action will be triggered in any pull request which has SQL files changed. It will call the SQL review service to check if the change is valid according to the SQL review rules.

### About parameters

- `database-type`: **Required**. The database type, should be one of `MYSQL`, `POSTGRES` or `TIDB`.
- `override-file-path`: **Optional**. Your SQL review rules configuration file path. You can configure and generate this file in [Bytebase SQL Review Guide](https://www.bytebase.com/sql-review-guide) page. You can ignore this parameter and only provide the `template-id` if you don't want to customize rules.
- `template-id`: **Optional**. The SQL Review rule template id, should be one of [`bb.sql-review.prod`](https://bytebase.com//sql-review-guide?templateId=bb.sql-review.prod) or [`bb.sql-review.dev`](https://bytebase.com//sql-review-guide?templateId=bb.sql-review.dev). You can ignore this parameter if you provide the `override-file-path` parameter.
- `file-pattern`: **Optional**. The file path regex pattern for your SQL files. Defaults `^.*\.sql$`. For example, if you only want to subscribe to the SQL file changes in the `db` folder, you can set this parameter to `^db/.*\.sql$`.

## Examples

Once you configure the action, you can get these error or warning message based on your SQL review rules:

![example](./assets/example.webp)

- [bytebase/sql-review-action-example](https://github.com/Bytebase/sql-review-action-example) shows a minimum example of configuring and overriding the SQL rules for multiple database systems.

- [bytebase/bytebase](https://github.com/Bytebase/Bytebase) shows an example of onboarding SQL Review action into a real world project.
  - Check this PR [chore: add sql review action](https://github.com/Bytebase/Bytebase/pull/2100) to see how to add this action into your repository.
  - Check this PR [chore: add test sql](https://github.com/Bytebase/Bytebase/pull/2177/files) to see how this action works in your pull request.
