# ExceptionHandler 
  - RestControllerTest
  ~~~
  package controllers.test;

import java.util.ArrayList;
import java.util.List;

import javax.persistence.EntityManager;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import models.dao.TestDao;
import models.entity.TestEntity;



@RestController
public class RestControllerTest {
	...
  
	@GetMapping("/rest2/{err}")
	public String test2(@PathVariable(name = "err", required = false ) String bol) {

		if (bol.equals("true")) {
			throw new RuntimeException("err");
			
		}
		
		return "정상처리";
	}
	
	@ExceptionHandler(RuntimeException.class)
	public String err(RuntimeException ex) {
		
		
		return ex.getMessage();
	}
  
	...
	
}

  ~~~
