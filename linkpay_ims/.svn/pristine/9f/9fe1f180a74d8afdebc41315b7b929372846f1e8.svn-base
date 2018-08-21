package egov.linkpay.ims.loginout.service;

import java.util.HashMap;
import java.util.Map;

import javax.annotation.Resource;
import javax.servlet.http.HttpSession;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Service;

import egov.linkpay.ims.common.common.CommonMessage;
import egov.linkpay.ims.common.common.CommonUtils;
import egov.linkpay.ims.loginout.dao.LogInOutDAO;

/**------------------------------------------------------------
 * Package Name   : egov.linkpay.ims.loginout.service
 * File Name      : LogInOutServiceImpl.java
 * Description    : LogIn/out Service Implement
 * Author         : ymjo, 2015. 10. 5.
 * Modify History : Just Created.
 ------------------------------------------------------------*/
@Service("logInOutService")
public class LogInOutServiceImpl implements LogInOutService {
    Logger logger = Logger.getLogger(this.getClass());
    
    @Resource(name="logInOutDAO")
    private LogInOutDAO logInOutDAO;
    
    @Override
    public Map<String, Object> selectIMSNotice() throws Exception {
        return logInOutDAO.selectIMSNotice();
    }
    
    @Override
    public Map<String, Object> selectAdminInfo(Map<String, Object> objMap) throws Exception {
    	int intUserStatus = 0;
    	Map<String,Object> objAdminInfoMap = logInOutDAO.selectAdminInfo(objMap);
        
        if(objAdminInfoMap == null){
            objMap.put("ResultMessage", CommonMessage.MSG_ERR_LOGIN_FAIL);
            throw new Exception(CommonMessage.MSG_ERR_LOGIN_FAIL);
        }
        
        intUserStatus = Integer.parseInt(objAdminInfoMap.get("STATUS").toString());
        
        if(intUserStatus != 1){
            objMap.put("ResultMessage", CommonMessage.MSG_ERR_LIMITED_ACCOUT);
            throw new Exception(CommonMessage.MSG_ERR_LIMITED_ACCOUT);
        }
        
        return objAdminInfoMap;
    }
    
    @Override
    public Map<String, Object> selectAdminInfo2(Map<String, Object> objMap, HttpSession session) throws Exception {
        Map<String,Object> objAdminInfoMap = logInOutDAO.selectAdminInfo2(objMap);
        
        if(objAdminInfoMap == null){	// worng account id
            objMap.put("ResultMessage", CommonMessage.MSG_ERR_LOGIN_FAIL);
            throw new Exception(CommonMessage.MSG_ERR_LOGIN_FAIL);
        }
        
        int failCnt = Integer.parseInt(objAdminInfoMap.get("FAIL_CNT").toString());	// over fail_cnt
        if(failCnt >= 5){ 	// check fail_count
        	
        	objMap.put("LOG_DESC", "Login Fail - over fail cnt");
            logInOutDAO.insertLoginLog(objMap);
        	
        	objMap.put("ResultMessage", CommonMessage.MSG_ERR_PSWD_FAIL_CNT);
            throw new Exception(CommonMessage.MSG_ERR_PSWD_FAIL_CNT);
        }
        
        if(!objMap.get("PSWD").equals(objAdminInfoMap.get("PSWD"))){	// check password
        	logInOutDAO.updateFailCnt(objMap);	// add fail_cnt
        	
        	objMap.put("LOG_DESC", "Login Fail - pswd");
            logInOutDAO.insertLoginLog(objMap);
        	
        	objMap.put("ResultMessage", CommonMessage.MSG_ERR_LOGIN_FAIL);
            throw new Exception(CommonMessage.MSG_ERR_LOGIN_FAIL);
    	}
        
        logInOutDAO.updateFailCntInit(objMap);		// init fail_cnt to 0
        
        String updDt = "";
        updDt = objAdminInfoMap.get("UPD_DT") == null ? objAdminInfoMap.get("REG_DT").toString() : objAdminInfoMap.get("UPD_DT").toString(); 
        updDt = updDt.substring(0, 8);
        
        long diff = CommonUtils.diffOfDate(updDt, CommonUtils.strToday());		// over date changed update
        if(diff >= 90){		// check 90 days 
        	objMap.put("ResultMessage", "90");
            throw new Exception(CommonMessage.MSG_ERR_PWSD_OVER_90);
        }
		
        objMap.put("LOG_DESC", "Login");
        logInOutDAO.insertLoginLog(objMap);
        
        session.setAttribute("USR_ID", objMap.get("USR_ID"));
        
        return objAdminInfoMap;
    }

    @Override
    public void updateAdminPSWD(Map<String, Object> objMap) throws Exception {
    	String newPswd = objMap.get("NEWPSWD").toString();
    	Map<String, Object> oldPwsd = new HashMap<String, Object>();
    	
    	oldPwsd = logInOutDAO.selectPswdHistory(objMap);
    	String pwsdHistory[] = oldPwsd.get("OLD_PW").toString().split(",");
    	
    	for(String oldPswd : pwsdHistory){	// check pswd history
    		if(newPswd.equals(oldPswd)){
    			objMap.put("ResultMessage", CommonMessage.MSG_ERR_PWSD_HISTORY);
                throw new Exception(CommonMessage.MSG_ERR_PWSD_HISTORY);
    		}
    	}
    	
    	objMap.put("OLD_PW1", pwsdHistory[0]);
    	objMap.put("OLD_PW2", pwsdHistory[1]);
    	objMap.put("OLD_PW3", pwsdHistory[2]);
    	objMap.put("OLD_PW4", pwsdHistory[3]);
    	objMap.put("OLD_PW5", pwsdHistory[4]);
    	
        logInOutDAO.updateAdminPSWD(objMap);		// change pswd and update old_pw1
    }
    
    @Override
    public void insertLogoutLog(Map<String, Object> objMap) throws Exception {
    	logInOutDAO.insertLogoutLog(objMap);
    }
}



