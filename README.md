# Sample-code
For get information.

## Convert server date to Display Date
	struct DateFormet {
		static let serverDateFormate = "yyyy-MM-dd HH:mm:ss"
		static let dateFormet 	     = "yyyy-MM-dd"
  	}
	
 	 func convertServerDateToDisplayDateFormet() -> String {
		let dateformet = DateFormatter()
    		dateformet.dateFormat = DateFormet.serverDateFormate
    		if(dateformet.date(from: self) != nil){
			let date = dateformet.date(from: self)
        		dateformet.dateFormat = DateFormet.dateFormet
        		return dateformet.string(from: date!)
     	}
     	return self
 	 }
	 
## Get time ago
	func getTimesAgo(_ date : Date) -> String {
        
        let units = Set<Calendar.Component>([.year, .month, .day, .weekOfYear])
        let components = Calendar.current.dateComponents(units, from: date as Date, to: Date())
        
        if components.year! > 0 {
            return ("\(components.year!)y ago")
            
        } else if components.month! > 0 {
            return ("\(components.month!)m ago")
            
        } else if components.weekOfYear! > 0 {
            return ("\(components.weekOfYear!)w ago")
            
        } else if (components.day! > 0) {
            return ("\(components.day!)d ago")
            
        } else {
            return "To day"
        }
	}
	
### NSDictionary key validation
	func KeyValidationFor_String(strKey: String) -> String {
		
		// CHECK FOR EMPTY
		if(self.allKeys.count == 0) {
			return String()
		}
		
		// CHECK IF KEY EXIST
		if let val = self.object(forKey: strKey) {
			if((val as AnyObject).isEqual(NSNull())) {
				return String()
			}
		} else {
			// KEY NOT FOUND
			return String()
		}
		
		// CHECK FOR NIL VALUE
		let aValue : AnyObject = self.object(forKey: strKey)! as AnyObject
		if aValue.isEqual(NSNull()) {
			return String()
		}
		else if(aValue.isKind(of: NSNumber.self)){
			return String(format:"%f", (aValue as! NSNumber).doubleValue)
		}
		else {
			
			if aValue is String {
				return self.object(forKey: strKey) as! String
			}
			else{
				return String()
			}
		}
	}

## Should Change Charater
	let set = CharacterSet(charactersIn: "ABCDEFGHIJKLMONPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ").inverted
	let set = CharacterSet(charactersIn: "@_.abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890-").inverted
            if string.rangeOfCharacter(from: set) != nil{
                shouldReturn = false
            }

## TextField 
import SkyFloatingLabelTextField

let MINIMUM_CHAR_PHONE_NUMBER = 6
let MAXIMUM_CHAR_PHONE_NUMBER = 10//13


enum AITextFieldType: Int {
    case Username
    case Email
    case PhoneNumber
    case Password
    case None
}

class AITextField: SkyFloatingLabelTextField {

    var stopKeyBoardToOpen:Bool = false     //to stop keyboard to be open
    var textFieldType: AITextFieldType = .None {
        didSet {
            switch textFieldType {
            case .Username:
                break
            case .Email:
                break
            case .PhoneNumber:
                break
            case .Password:
                self.isSecureTextEntry = true
                self.rightViewMode = .always
                let rightButton = UIButton(type: .custom)
                rightButton.setImage(UIImage(named: "Hide"), for: .normal)
                rightButton.contentVerticalAlignment = .bottom
                rightButton.frame = CGRect(x: CGFloat(self.frame.size.width - 25), y: 5, width: 25, height: 25)
                rightButton.addTarget(self, action: #selector(hideShowPassword(sender:)), for: .touchUpInside)
                self.rightView = rightButton
                break
            case .None:
                break
            }
        }
    }
    override init(frame: CGRect) {
        super.init(frame: frame)
        commitUI()
    }
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        commitUI()
    }
    
    override func awakeFromNib() {
        super.awakeFromNib()
        
    }
    
    func commitUI() {
        let color               = Colors.APP_BLUE_DARK_TEXT
        self.textColor          = color
        self.tintColor          = color
        self.lineColor          = Colors.APP_LINE
        self.selectedLineColor  = color
        self.placeholderColor   = Colors.APP_GRAY_PH
        self.titleColor         = Colors.APP_GRAY_PH
        self.selectedTitleColor = Colors.APP_GRAY_PH
        self.disabledColor      = color
        self.errorColor         = Colors.APP_RED
        self.titleFormatter = {
            $0.capitalizingFirstLetter()
        }
        self.lineHeight         = 2
        self.font = UIFont.regulerFont(ofSize: (self.font?.pointSize)!)
        self.delegate = self
    }

    func setTextFieldKeybord(textField: AITextField) {
        
        self.keyboardType = .default
        
        switch textFieldType {
        case .Username:
            self.autocorrectionType = .no
            self.keyboardType = .asciiCapable
            break
        case .Email:
            self.keyboardType = .emailAddress
            break
        case .PhoneNumber:
            self.keyboardType = .phonePad
            break
        case .Password:
            break
        case .None:
            break
        }
    }
    
    @objc func hideShowPassword(sender: UIButton) {
        self.isSecureTextEntry = !self.isSecureTextEntry
        sender.setImage(UIImage(named: (self.isSecureTextEntry ? "Hide" : "Show")), for: .normal)
    }
}

//MARK:- TEXTFILED DELEGET
extension AITextField: UITextFieldDelegate {
    
    func textFieldDidBeginEditing(_ textField: UITextField) {
        if let skyTextField = textField as? AITextField {
            skyTextField.errorMessage = ""
            setTextFieldKeybord(textField: textField as! AITextField)
//            if skyTextField.textFieldType == .EmailPhone {
//                self.lineColor = Colors.APP_LINE
//            }
        }
    }
    
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        self.endEditing(true)
        return true
    }

    func textFieldShouldBeginEditing(_ textField: UITextField) -> Bool {
        
        setTextFieldKeybord(textField: textField as! AITextField)
        return !self.stopKeyBoardToOpen
    }
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        let fullText = (textField.text! as NSString).replacingCharacters(in: range, with: string)
        var shouldReturn: Bool = true
        
        if textFieldType == .PhoneNumber {
            var result = true
            let set = CharacterSet(charactersIn: "0123456789").inverted
            if string.rangeOfCharacter(from: set) != nil{
                result = false
            }
            
            if(result == false || string == " " || fullText.count > MAXIMUM_CHAR_PHONE_NUMBER) {
                shouldReturn = false
            }
        }
        return shouldReturn
    }
}

//.pem file 
 - openssl pkcs12 -in /Users/atologistmac3/Downloads/yag_APNs_Distribution_Certi.p12 -out pushcert_yag.pem -nodes -clcerts
