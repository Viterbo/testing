# excepción  
* WELD-001408: Unsatisfied dependencies for type JsonObject with qualifiers @Default  

```
13:34:48,856 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-5) MSC000001: Failed to start service jboss.deployment.unit."mimontevideo-backend-0.0.1.war".WeldStartService: org.jboss.msc.service.StartException in service jboss.deployment.unit."mimontevideo-backend-0.0.1.war".WeldStartService: Failed to start service
	at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1904)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
Caused by: org.jboss.weld.exceptions.DeploymentException: WELD-001408: Unsatisfied dependencies for type JsonObject with qualifiers @Default
  at injection point [UnbackedAnnotatedField] @Inject private uy.gub.imm.mobile.mimontevideo.rs.avisos.DispositivosUsuario.authenticationData
  at uy.gub.imm.mobile.mimontevideo.rs.avisos.DispositivosUsuario.authenticationData(DispositivosUsuario.java:0)

	at org.jboss.weld.bootstrap.Validator.validateInjectionPointForDeploymentProblems(Validator.java:359)
	at org.jboss.weld.bootstrap.Validator.validateInjectionPoint(Validator.java:281)
	at org.jboss.weld.bootstrap.Validator.validateGeneralBean(Validator.java:134)
	at org.jboss.weld.bootstrap.Validator.validateRIBean(Validator.java:155)
	at org.jboss.weld.bootstrap.Validator.validateBean(Validator.java:518)
	at org.jboss.weld.bootstrap.ConcurrentValidator$1.doWork(ConcurrentValidator.java:68)
	at org.jboss.weld.bootstrap.ConcurrentValidator$1.doWork(ConcurrentValidator.java:66)
	at org.jboss.weld.executor.IterativeWorkerTaskFactory$1.call(IterativeWorkerTaskFactory.java:63)
	at org.jboss.weld.executor.IterativeWorkerTaskFactory$1.call(IterativeWorkerTaskFactory.java:56)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
	at org.jboss.threads.JBossThread.run(JBossThread.java:320)
```

# Una pregunta de alguien que tiene un problema muy similar  
https://stackoverflow.com/questions/28352461/weld-001408-unsatisfied-dependencies-for-type-customer-with-qualifiers-default

# Mi Código

```java
package uy.gub.imm.mobile.mimontevideo.rs.avisos;

import javax.inject.Inject;
import javax.json.JsonObject;
import javax.ws.rs.Consumes;
import javax.ws.rs.DELETE;
import javax.ws.rs.OPTIONS;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

import uy.gub.imm.mobile.mimontevideo.dto.DispositivoUsuario;
import uy.gub.imm.mobile.mimontevideo.mock.DispositivoUsuariosMock;
import uy.gub.imm.oauth2.utils.CheckBearerToken;
import uy.gub.imm.oauth2.utils.CheckBearerTokenInfo;

@Path("/dispositivosUsuario")
public class DispositivosUsuario {
	
	@Inject
	@CheckBearerTokenInfo
	private JsonObject authenticationData;	
	
	...
  
	@POST
	@Consumes({MediaType.APPLICATION_JSON})
	@Produces({MediaType.APPLICATION_JSON})
	@CheckBearerToken(scope = "profile")
	public Response agregarDispositivoUsuario(DispositivoUsuario dispositivoUsuario){
		System.out.println("authenticationData: " + authenticationData.toString());
		DispositivoUsuariosMock.getInstance().agregarDispositivo(
            dispositivoUsuario.getIdUsuario(),
            dispositivoUsuario.getDispositivoUsuario());
		return Response.ok()
	    	      .header("Access-Control-Allow-Origin", "*")
	    	      .header("Access-Control-Allow-Methods", "POST, GET, PUT, UPDATE, OPTIONS")
	    	      .header("Access-Control-Allow-Headers", "Content-Type, Accept, X-Requested-With")
	    	      .build();
	}
	
    ...

}
```



