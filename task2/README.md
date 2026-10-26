# pseudo-code

// CASE 1: Employee Submits Request

    SubmitVacationRequest(employee, dates, category){
        IF (ValidateRules(employee, dates, category) == FALSE)
            RETURN "Error: Request does not pass rules."
        
        newRequest = CreateRequest(employee, dates, category, vacation_status_id="PENDING")
        SaveToDatabase(newRequest)
        
        IF (employee.RequiresManagerApproval == TRUE)
            SendEmail(to=employee.Manager, subject="Approval Needed", link=newRequest.Request_id)
        ELSE{
            // High-level employees skip manager approval
            newRequest.vacation_status_id = "HR_PENDING"
            SaveToDatabase(newRequest)
            SendEmail(to="HR_Team", subject="HR Validation Needed")
            }
    }
  

// CASE 2: Manager/HR Approves 

    ProcessApproval(request_id, actor, decision){
        request = GetRequestFromDatabase(request_id)
        
        IF (actor.role == "MANAGER" AND request.vacation_status_id == "PENDING"){
            IF (decision == "APPROVE"){
                request.vacation_status_id = "HR_PENDING" // NEW STATE
                SendEmail(to="HR_Team", subject="Request needs HR Validation")
                }
            ELSE IF (decision == "DENY"){
                request.vacation_status_id = "REJECTED"
                SendEmail(to=request.Employee_id, subject="Request Rejected")
                }
          }
                
        ELSE IF (actor.role == "HR_CLERK" AND request.vacation_status_id == "HR_PENDING"){
            IF (decision == "APPROVE"){
                request.vacation_status_id = "HR_APPROVAL" 
                SendEmail(to=request.Employee_id, subject="Request Fully Approved")
                }
            ELSE IF (decision == "DENY"){
                request.vacation_status_id = "REJECTED"
                SendEmail(to=request.Employee_id, subject="Request Rejected by HR")
                }
          }
            
        SaveToDatabase(request)
      }


# state machine

<img width="1141" height="631" alt="image" src="https://github.com/user-attachments/assets/f7917dae-c684-4748-b47a-2fd30252c168" />

# sequence diagram

<img width="1335" height="951" alt="image" src="https://github.com/user-attachments/assets/a900a457-b801-4d2e-8102-bdb2feb58b53" />

# Flowchart

<img width="704" height="917" alt="image" src="https://github.com/user-attachments/assets/2148381c-7b39-443f-941c-0e94f657ac8f" />

# sequence diagram

<img width="911" height="415" alt="image" src="https://github.com/user-attachments/assets/da7e69fd-0fbe-4bd8-bb6f-d6295ca346e2" />

