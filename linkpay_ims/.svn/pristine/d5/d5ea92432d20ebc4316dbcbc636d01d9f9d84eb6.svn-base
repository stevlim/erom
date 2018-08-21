package egov.linkpay.ims.baseinfomgmt.service;

import java.util.List;
import java.util.Map;

public interface BaseInfoMgmtService
{
	//기본정보조회 
	Map<String,Object> baseInfo(Map< String, Object > paramMap) throws Exception;
	//기본정보조회-관리자 권한 조회 
	List<Map<String,Object>> selectEmpAuthSearch(Map< String, Object > paramMap) throws Exception;
	//기본정보조회 list
	List<Map<String,Object>> selectBaseInfoList(Map< String, Object > paramMap) throws Exception;
	int selectBaseInfoListTotal(Map< String, Object > paramMap) throws Exception;
	//기본정보조회 vid mid 정보
	List<Map<String,Object>> selectVMid(Map< String, Object > paramMap) throws Exception;
	
	// 다량등록 결과 리스트
	List<Map<String,Object>> insertMultiRegist(Map< String, Object > paramMap) throws Exception;
}
