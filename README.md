# SimpleInjector.WebApi.Middleware

Este pacote contém um middleware que encapsula a implementação do Dependence Resolver utilizado pela classe HttpConfiguration

1º - Instalando o pacote

https://www.nuget.org/packages/SimpleInjector.Integration.WebApi.Extensions/1.0.0-alpha

```bash
Install-Package SimpleInjector.Integration.WebApi.Extensions -Pre
```
2º - Classe StartUp com a implementação do middleware

```C#

public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            if (app == null) throw new ArgumentNullException("app");

            // CORS
            app.UseCors(CorsOptions.AllowAll);

            // Web API
            var httpConfig = new HttpConfiguration();
                        
            WebApiConfig.Register(httpConfig);
            
            var container = CreateContainer(app);

            app.UseSimpleInjector(container, httpConfig);

            httpConfig.EnsureInitialized();
            app.UseWebApi( httpConfig);

        }

        private Container CreateContainer(IAppBuilder app)
        {
            var container = new Container();

            container.RegisterSingle<IAppBuilder>(app);                      

            container.Register<IUnitOfWork, KeretaUnitOfWork>();
            container.Register<IRepository<Modelo>, Repository<Modelo>>();
            container.Register<IRepository<Processo>, Repository<Processo>>();
            container.Register<IRepository<Gravidade>, Repository<Gravidade>>();
            container.Register<IRepository<SubSistema>, Repository<SubSistema>>();
            container.Register<IRepository<Sistema>, Repository<Sistema>>();
            container.Register<IRepository<Marca>, Repository<Marca>>();
            container.Register<IRepository<CentroDeCusto>, Repository<CentroDeCusto>>();
            container.Register<IRepository<FuncaoDoColaborador>, Repository<FuncaoDoColaborador>>();
            container.Register<IRepository<Colaborador>, Repository<Colaborador>>();
            container.Register<IRepository<Veiculo>, Repository<Veiculo>>();
            container.Register<IRepository<Categoria>, Repository<Categoria>>();

            container.Verify();

            return container;
        }
    }
```

3º - Dependências de Pacotes

```xml
<packages>
  <package id="Microsoft.AspNet.Cors" version="5.2.3" targetFramework="net451" />
  <package id="Microsoft.AspNet.WebApi.Client" version="5.2.3" targetFramework="net451" />
  <package id="Microsoft.AspNet.WebApi.Core" version="5.2.3" targetFramework="net451" />
  <package id="Microsoft.AspNet.WebApi.Cors" version="5.2.3" targetFramework="net451" />
  <package id="Microsoft.AspNet.WebApi.Owin" version="5.2.3" targetFramework="net451" />
  <package id="Microsoft.Owin" version="3.0.1" targetFramework="net451" />
  <package id="Microsoft.Owin.Cors" version="3.0.1" targetFramework="net451" />
  <package id="Microsoft.Owin.Diagnostics" version="3.0.1" targetFramework="net451" />
  <package id="Microsoft.Owin.Host.HttpListener" version="3.0.1" targetFramework="net451" />
  <package id="Microsoft.Owin.Hosting" version="3.0.1" targetFramework="net451" />
  <package id="Microsoft.Owin.SelfHost" version="3.0.1" targetFramework="net451" />
  <package id="Newtonsoft.Json" version="6.0.4" targetFramework="net451" />
  <package id="Owin" version="1.0" targetFramework="net451" />
  <package id="SimpleInjector" version="2.7.3-alpha1" targetFramework="net451" />
  <package id="SimpleInjector.Extensions.ExecutionContextScoping" version="2.7.3-alpha1" targetFramework="net451" />
  <package id="SimpleInjector.Integration.WebApi" version="2.7.3-alpha1" targetFramework="net451" />
</packages>
```
