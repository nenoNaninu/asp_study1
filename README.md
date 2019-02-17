# asp_study1
Azureに実際にデプロイするところまで。

# やり方
基本的には
[速習 ASP.NET Core](https://www.amazon.co.jp/dp/B078CXYZ6L/ref=cm_sw_em_r_mt_dp_U_RtzACb2RQMWG5)
をベースに。
ただし、dotnet core cliでのスキャフォールディングは若干違う。
```
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
```
などとしてスキャフォールディングを行う。詳細は[公式ページ](https://docs.microsoft.com/ja-jp/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-2.2&tabs=visual-studio-code)

databaseなどのコマンドは[公式ページ](https://docs.microsoft.com/ja-jp/ef/core/managing-schemas/migrations/)
に書いてある
```
dotnet ef migrations add InitialCreate
dotnet ef database update
```
で基本的にはokだか、azureにデプロイする際には
```
dotnet ef migrations add InitialCreate
```
をしたあとにはマイグレーションで生成されたInitialCreate.csの中にあるIdを若干いじらないと行けない。
```
Id = table.Column<int>(nullable: false)
    .Annotation("Sqlite:Autoincrement", true)
    .Annotation("SqlServer:ValueGenerationStrategy", SqlServerValueGenerationStrategy.IdentityColumn),
```
最後の行がないと、SqlServerを使ったときにインサートエラーが出て死ぬ。おそらくmigrationコマンドを叩くときに
```
-d
--data-annotations
```
のどちらかのコマンドを叩けばいいのだが調べがついていない。オプションの詳細については[こちら](https://docs.microsoft.com/ja-jp/ef/core/miscellaneous/cli/dotnet)

Azureサイドの設定などは[こちら](https://docs.microsoft.com/ja-jp/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)が非常によい。
