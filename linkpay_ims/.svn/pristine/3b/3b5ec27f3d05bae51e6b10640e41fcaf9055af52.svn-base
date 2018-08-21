<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ include file="../tilesLayouts/commonPageLibrary.jsp" %>

<script type="text/javascript">
$(document).ready(function() {
    fnInitEvent();
    //fnLoginProc();
});

function fnInitEvent() {
	$("#USR_ID").focus();

	fnSetValidate();
	fnSetPWValidate();

	$("#USR_ID").on("keydown", function(event) {
        if (event.which == 13 && $("#frm").valid()) {
        	fnLoginProc();
        }
    });

	$("#PSWD").on("keydown", function(event) {
        if (event.which == 13 && $("#frmpw2").valid()) {
        	fnChangePwProc();
        }
    });

	$("#btnLogin").on("click", function() {
		if (!$("#frm").valid()) {
            return;
        }

		fnLoginProc();
	});

	$("#btnChangePW2").on("click", function(){
		if (!$("#frmpw2").valid()) {
            return;
        }

		fnChangePwProc();
	});

	fnLoadID();

	fnSessionExpiredMsg();
}

function fnSetValidate() {
    var arrValidate = {
                FORMID   : "frm",
                VARIABLE : {
                	"USR_ID"  : {minlength:4, maxlength: 10, required: true},
                	"PSWD"    : {minlength:6, maxlength: 20, required: true}
                    }
    }

    IONPay.Utils.fnSetValidate(arrValidate);
}

function fnSetPWValidate() {
    var arrValidate = {
                FORMID   : "frmpw2",
                VARIABLE : {
                	USR_ID	  : {minlength:4, maxlength: 10, required: true},
                    CURPSWD   : {minlength:6, maxlength: 20, required: true},
                    NEWPSWD   : {minlength:6, maxlength: 20, notEqualTo: "#CURPSWD", required: true, passwordCheck:true, pwCheckConsecChars:true },
                    RENEWPSWD : {minlength:6, maxlength: 20, equalTo: "#NEWPSWD", required: true, passwordCheck:true, pwCheckConsecChars:true }
                }
    }

    IONPay.Utils.fnSetValidate(arrValidate);
}

function fnLoginProc() {
	var $USR_ID = $("#USR_ID");
	var $PSWD   = $("#PSWD");

	if ($.trim($USR_ID.val()) == "" || $.trim($PSWD.val()) == "") {
	    IONPay.Msg.fnAlert("Please Enter Your Username or Password.");
	} else {
		arrParameter = {
		           "USR_ID" : $USR_ID.val(),
		           "PSWD"   : $PSWD.val()
		           };

		strCallUrl  = "/logInProc.do";
		strCallBack = "fnLoginProcRet";

		IONPay.Ajax.fnRequest(arrParameter, strCallUrl, strCallBack);
	}
}

function fnLoginProcRet(objJson) {
    if (objJson.resultCode == 0) {
        if ($("input:checkbox[id='save_id']").is(":checked") == true) {
            IONPay.Utils.fnSetCookie("IMSSaveID=", $("#USR_ID").val(), 3650);
        } else {
            IONPay.Utils.fnSetCookie("IMSSaveID=", $("#USR_ID").val(), 0);
        }

        var strPrevPaage = "${sessionScope['IMSPrevPage']}";

        if($.trim(strPrevPaage) == "") {
            location.href = "/home/dashboard/dashboard.do";
        } else {
            location.href = strPrevPaage;
        }
    } else {
    	if(objJson.resultMessage == "90"){
    		$("body").addClass("breakpoint-1024 pace-done modal-open ");
            $("#btnModalPW").click();
    	} else {
    		IONPay.Msg.fnAlert(objJson.resultMessage);
    	}
    }
}

function fnLoadID(){
    var strSaveID = IONPay.Utils.fnGetCookie("IMSSaveID=");

    if (strSaveID != "") {
        $("#USR_ID").val(strSaveID);
        $("#save_id").attr("checked", true);
    } else {
        $("#save_id").attr("checked", false);
    }
}

function fnSessionExpiredMsg() {
	if (IONPay.Utils.fnGetCookie("<c:out value="${CommonConstants.IMS_SESSION_EXP_KEY}"/>") == "=true") {
	    IONPay.Utils.fnSetCookie("<c:out value="${CommonConstants.IMS_SESSION_EXP_KEY}"/>=", "false", 0);
		IONPay.Msg.fnAlert(gMessage("IMS_COM_MSG_0002"));
	}
}

function fnChangePwProc(){

	var $USR_ID = $("#USR_ID");
	var $CURPSWD = $("#CURPSWD");
	var $NEWPSWD = $("#NEWPSWD");
	var $RENEWPSWD = $("#RENEWPSWD");

	if ($.trim($USR_ID.val()) == "" || $.trim($CURPSWD.val()) == "" || $.trim($NEWPSWD.val()) == "" || $.trim($RENEWPSWD.val()) == "" ) {
		IONPay.Msg.fnAlert("Please Enter Your ID, Current Password, NewPassword and RenewPassword.");
	} else if($NEWPSWD.val() != $RENEWPSWD.val()){
		IONPay.Msg.fnAlert("New password and Renew password art wrong. Check please.");
	} else {
		arrParameter = {
        	"USR_ID" 	: $USR_ID.val(),
        	"CURPSWD"   : $CURPSWD.val(),
        	"NEWPSWD"	: $NEWPSWD.val(),
        };
		strCallUrl  = "/logInChangePasswordBeforeProc.do";
		strCallBack = "fnlogInChangePasswordBeforeProcRet";

		IONPay.Ajax.fnRequest(arrParameter, strCallUrl, strCallBack);
	}

}

function fnlogInChangePasswordBeforeProcRet(objJson){
	if (objJson.resultCode == 0) {
		IONPay.Utils.fnFrmReset("frmpw2");
    	IONPay.Msg.fnAlertWithModal(gMessage('IMS_COM_LNB_0001'), "PWModal", true);
	} else {
		IONPay.Utils.fnFrmReset("frmpw2");
	    IONPay.Msg.fnAlertWithModal(objJson.resultMessage, "PWModal", true);
	}
}


</script>
	<form id="frm">
		<div class="link-logo">
	    	<img src="//<spring:eval expression="@config['SERVICE_DOMAIN']"/>/Bootstrap/assets/img/LINK%20PAY-LOGO.png" alt="logo" />
		</div>
		<div class="login-block">
		    <h1>Login</h1>
		    <input type="text" placeholder="Username" id="USR_ID" name="USR_ID" value="linkpay" style="padding:0 20px 0 50px !important"/>
		    <input type="password" placeholder="Password" id="PSWD" name="PSWD" value="123456" style="padding:0 20px 0 50px !important"/>
		    <button id="btnLogin" class="btn1">Submit</button>
		</div>
	</form>


    <button id="btnModalPW" data-toggle="modal" data-target="#PWModal" style="width:0px; height:0px; display:none;"></button>
    <div class="modal fade" id="PWModal" tabindex="-1" role="dialog" aria-labelledby="modalPW" aria-hidden="true">
        <div class="modal-dialog">
        <form id="frmpw2">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-hidden="true" onclick="IONPay.Msg.fnResetBodyClass(); IONPay.Utils.fnFrmReset('frmpw2');">×</button>
                    <br>
                    <i class="fa fa-unlock-alt fa-2x"></i>
                    <h4 id="modalPW" class="semi-bold"><spring:message code='IMS_COM_LNB_0006'/></h4>
                    <br>
                </div>
                <div class="modal-body">
                    <div class="row form-row">
                        <div class="col-md-12">
                            <div class="input-with-icon  right">
                                <i class=""></i>
                                <input class="form-control pswd" type="text" id="USR_ID" name="USR_ID" placeholder="<spring:message code='IMS_AM_MM_0016'/>" maxlength=64">
                            </div>
                        </div>
                    </div>
                    <div class="row form-row">
                        <div class="col-md-12">
                            <div class="input-with-icon  right">
                                <i class=""></i>
                                <input class="form-control pswd" type="password" id="CURPSWD" name="CURPSWD" placeholder="<spring:message code='IMS_COM_LNB_0003'/>" maxlength=64">
                            </div>
                        </div>
                    </div>
                    <div class="row form-row">
                        <div class="col-md-12">
                            <div class="input-with-icon  right">
                                <i class=""></i>
                                <input class="form-control pswd" type="password" id="NEWPSWD" name="NEWPSWD" placeholder="<spring:message code='IMS_COM_LNB_0004'/>" maxlength=64">
                            </div>
                        </div>
                    </div>
                    <div class="row form-row">
                        <div class="col-md-12">
                            <div class="input-with-icon  right">
                                <i class=""></i>
                                <input class="form-control pswd" type="password" id="RENEWPSWD" name="RENEWPSWD" placeholder="<spring:message code='IMS_COM_LNB_0005'/>" maxlength=64">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" id="btnChangePW2" class="btn btn-danger">&nbsp&nbspOk&nbsp&nbsp</button>
                    <button type="button" class="btn btn-default" data-dismiss="modal" onclick="IONPay.Msg.fnResetBodyClass(); IONPay.Utils.fnFrmReset('frmpw2');">Cancel</button>
                </div>
            </div>
        </form>
        </div>
    </div>
