# ControllerAdvice (컨트롤러 공통 실행부분 처리)
  - CommonErrorHandeler
  ~~~
  @ControllerAdvice(적용될 패키지 지정(가변 매개변수))
    public class CommonErrorHandler {

	public String handler(RuntimeException ex,Model model,HttpServletResponse response) {
		
   ...
   
		return 이동할 페이지;  //오류페이지 이동
	}
	
}

  ~~~


  ex)
  ~~~
  @ControllerAdvice({"controllers","controller.member"}) //적용될 패키지 지정
  public class CommonErrorHandler {

	@ExceptionHandler(RuntimeException.class)	//각페이지 오류 공통 처리 부분
	public String handler(RuntimeException ex,Model model,HttpServletResponse response) {
		
    /**오류 처리 S*/
    
		if (ex instanceof CommonException) {
			Map<String, Object> errorInfo=((CommonException)ex).gets();
			int statusCode=(Integer)errorInfo.get("statusCode");
			if (statusCode >0) response.setStatus(statusCode);
			
			model.addAllAttributes(errorInfo);

		}
		
		model.addAttribute("message",ex.getMessage());
		ex.printStackTrace();
		
		/**오류 처리 E*/

		
		return "common/error";  //오류페이지 이동
	}
	
}

  ~~~
