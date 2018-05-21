# QRCodeGenerating
Here you can learn how to create QRCode in simple steps.
Here you can create QRCode by using CIFilter  

you no need to import any framwork ,Directly create CIFilter Object

import UIKit
import AVFoundation

class ViewController: UIViewController {

    @IBOutlet weak var QRCodeImage: UIImageView!
    var strQRCodeValue = NSString()
    var filter = CIFilter()
    
    //Here QRCodeImage is an image view which placed on my story board.
    //strQRCode value is a string value to store in QRCode
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        strQRCodeValue = "371635452675475" //This is my sample value you can Give what value you want to store in QRCode
        
    }
     @IBAction func btnQRCodeGeneate(_ sender: Any) {
        
        //Convert Your string value to data by using isoLatin1 Encoding Which is prefered from apple
        
        let dataQRCode = strQRCodeValue.data(using: String.Encoding.isoLatin1.rawValue, allowLossyConversion: false)
        
        //Give the Filter what type you want
        
        filter = CIFilter(name:"CIQRCodeGenerator")!
        //If you want to create BarCode use "CICode128BarcodeGenerator" insted of "CIQRCodeGenerator"
        
        guard let colorFilter = CIFilter (name:"CIFalseColor") else {
            return
        }
         filter.setValue(dataQRCode, forKey: "inputMessage")
        
        //If you want you can set the color of QRCode Here
        filter.setValue("H", forKey:"inputCorrectionLevel")
        colorFilter.setValue(filter.outputImage, forKey: "inputImage")
        colorFilter.setValue(CIColor(red: 1, green: 1, blue: 1), forKey: "inputColor1") // Background white
        colorFilter.setValue(CIColor(red: 1, green: 0, blue: 0), forKey: "inputColor0") // Foreground or the barcode RED
        
        
        guard let qrCodeImage = colorFilter.outputImage
            else {
                return
        }
        
        
        //Set Transformation of QRCode
        
        let scaleX = QRCodeImage.frame.size.width / qrCodeImage.extent.size.width
        let scaleY = QRCodeImage.frame.size.height / qrCodeImage.extent.size.height
        let transform = CGAffineTransform(scaleX: scaleX, y: scaleY)
     
        //Show QRCode on ImageView
        if let output = colorFilter.outputImage?.transformed(by: transform) {
            
             QRCodeImage.image = UIImage(ciImage: output)
        }
        
    }
    
}
