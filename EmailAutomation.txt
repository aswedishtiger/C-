using System;
using System.Net;
using System.Net.Mail;

class EmailAutomation
{
    static void SendEmail(string recipient, string subject, string body, string attachmentPath = null)
    {
        MailMessage mail = new MailMessage();
        SmtpClient smtpServer = new SmtpClient("smtp.gmail.com");

        mail.From = new MailAddress("your_email@gmail.com");
        mail.To.Add(recipient);
        mail.Subject = subject;
        mail.Body = body;

        if (attachmentPath != null)
        {
            Attachment attachment = new Attachment(attachmentPath);
            mail.Attachments.Add(attachment);
        }

        smtpServer.Port = 587;
        smtpServer.Credentials = new NetworkCredential("your_email@gmail.com", "your_password");
        smtpServer.EnableSsl = true;

        smtpServer.Send(mail);
        Console.WriteLine($"Email sent to {recipient}.");
    }

    static void Main(string[] args)
    {
        string recipient = "recipient@example.com";
        string subject = "Automated Email";
        string body = "This is an automated email.";
        string attachmentPath = @"C:\path\to\file.pdf";
        
        SendEmail(recipient, subject, body, attachmentPath);
        Console.WriteLine("Emails sent successfully.");
    }
}