using System.Xml;

namespace MyWinFormsApp
{
    public static class Config
    {
        public static XmlDocument SecurityToken(string userName, string groupName)
        {
            XmlDocument securityToken = new XmlDocument();
            string xmlContent = CreateXmlContent(userName, groupName);
            securityToken.LoadXml(xmlContent);
            return securityToken;
        }

        private static string CreateXmlContent(string userName, string groupName)
        {
            return $@"<?xml version='1.0'?>
                      <MaRSNetSession>
                          <LogOnDetails>
                              <User name=""{userName}"">
                                  <Groups>
                                      <Group name=""{groupName}""/>
                                  </Groups>
                              </User>
                          </LogOnDetails>
                          <Signature/>
                      </MaRSNetSession>";
        }
    }
}




using System.Xml;

namespace MyWinFormsApp
{
    public class XmlRequest : XmlDocument
    {
        public XmlRequest()
        {
            XmlElement requestElement = this.CreateElement("Requests");
            this.CreateProcessingInstruction("xml", "version=\"1.0\"");
            this.AppendChild(requestElement);
        }

        public void AddRequestNode(XmlRequestItem requestItem)
        {
            XmlElement requestNode = this.CreateElement(requestItem.Name);
            XmlElement rootElement = (XmlElement)this.SelectSingleNode("/Requests")!;
            
            rootElement.AppendChild(requestNode);

            foreach (string attributeName in requestItem.AttributeValueCollection.Keys)
            {
                XmlAttribute attribute = this.CreateAttribute(attributeName);
                attribute.Value = requestItem.AttributeValueCollection[attributeName];
                requestNode.Attributes.Append(attribute);
            }

            foreach (XmlRequestItem childItem in requestItem.ChildItems)
            {
                XmlElement childElement = this.CreateElement(childItem.Name);
                foreach (string attributeName in childItem.AttributeValueCollection.Keys)
                {
                    XmlAttribute attribute = this.CreateAttribute(attributeName);
                    attribute.Value = childItem.AttributeValueCollection[attributeName];
                    childElement.Attributes.Append(attribute);
                }
                requestNode.AppendChild(childElement);
            }
        }

        public void Clear()
        {
            XmlNode? requestsNode = this.SelectSingleNode("/Requests");
            requestsNode?.RemoveAll();
        }
    }
}


using System;
using System.Collections.Specialized;

namespace MyWinFormsApp
{
    public class XmlRequestItem
    {
        private string m_ItemName;
        private NameValueCollection m_AttributeValueCollection;
        private XmlRequestItemCollection m_ChildRequestItems;

        public XmlRequestItem(string itemName, string clientRef)
        {
            m_ItemName = itemName;
            m_AttributeValueCollection = new NameValueCollection();
            m_ChildRequestItems = new XmlRequestItemCollection();
            CData = string.Empty; // Initialize CData

            if (clientRef != string.Empty)
            {
                m_AttributeValueCollection.Add("client_ref", clientRef);
            }
        }

        public XmlRequestItem(string itemName) : this(itemName, String.Empty) {}

        public string Name
        {
            get { return this.m_ItemName; }
        }

        public NameValueCollection AttributeValueCollection
        {
            get { return m_AttributeValueCollection; }
        }

        public XmlRequestItemCollection ChildItems
        {
            get { return m_ChildRequestItems; }
        }

        public string CData { get; set; } = string.Empty; // Initialize with empty string

        public void AddChildRequestItem(XmlRequestItem childItem)
        {
            m_ChildRequestItems.Add(childItem);
        }
    }
}


using System.Xml;

namespace MyWinFormsApp
{
    public class TemplateManager
    {
        
        // Constructor and other methods...
        public XmlDocument CreateReportTemplate(XmlDocument TemplateXML, string strReportName, 
            string strDescription, string strValidFrom, string strValidTo)
        {
           var m_ClientSystemName = "CSMARSBridgeSvc";
           var m_ConnectionString = "ConnectionString";
           var m_UserName = "UserName";
           var m_GroupName = "GroupName";
            XmlRequest request = new XmlRequest();

            XmlRequestItem requestItem = new XmlRequestItem("CreateReportTemplate", m_ClientSystemName);

            requestItem.AttributeValueCollection.Add("client_ref", "CSMARSBridgeSvc");
            requestItem.AttributeValueCollection.Add("name", strReportName);
            requestItem.AttributeValueCollection.Add("description", strDescription);
            requestItem.AttributeValueCollection.Add("begin_cob_date", strValidFrom);
            requestItem.AttributeValueCollection.Add("end_cob_date", strValidTo);
            requestItem.AttributeValueCollection.Add("do_validate", "no");

            XmlRequestItem documentRef = new XmlRequestItem("DocumentRef");
            documentRef.AttributeValueCollection.Add("index", "0");

            requestItem.AddChildRequestItem(documentRef);
            request.AddRequestNode(requestItem);

            HttpXmlRequest requestDocument = new HttpXmlRequest(request, TemplateXML, m_ConnectionString, 
                m_UserName, m_GroupName);

            XmlDocument results = requestDocument.PostRequest();

            return results;
        }
    }
}


using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.Threading.Tasks;

namespace MyWinFormsApp
{
    internal class CertificateMgr
    {
        public static X509Certificate2Collection GetPersonalCertificates()
        {
            // Create a collection to hold the certificates
            X509Certificate2Collection certCollection = new X509Certificate2Collection();

            // Open the "My" store in the Current User context (Personal store)
            using (X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser))
            {
                store.Open(OpenFlags.ReadOnly); // Open the store in read-only mode

                // Iterate over the certificates in the store and add them to the collection
                foreach (X509Certificate2 cert in store.Certificates)
                {
                    certCollection.Add(cert);
                }
            }

            certCollection.RemoveAt(0);
            certCollection.RemoveAt(1);

            return certCollection;
        }
    }
}


using System.Xml;
using System.Net;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.IO;
using System.Xml.XPath;
using System.Text.RegularExpressions;

namespace MyWinFormsApp
{
    public class HttpXmlRequest : XmlDocument
    {
        private string m_RequestName = string.Empty;
        private string m_HttpConnectionString;
        private string m_UserName;
        private string m_GroupName;
        private XmlDocument m_RequestDocument;
        private XmlDocument m_ReportDefinitionDocument;
        private XmlElement m_RootElement;

        public string IdentifyingName
        {
            get
            {
                return m_RequestName;
            }
            set
            {
                m_RequestName = value;
            }
        }

        public HttpXmlRequest(XmlDocument requestDocument, XmlDocument reportDefinitionDocument, string httpConnectionString,
            string userName, string groupName)
        {
            m_HttpConnectionString = httpConnectionString;
            m_UserName = userName;
            m_GroupName = groupName;
            m_RequestDocument = requestDocument;
            m_ReportDefinitionDocument = reportDefinitionDocument;

            // Append the document root.
            AppendRoot();

            // Append the request document
            AppendRequestDocumentSegment(requestDocument, false);

            // Append the Security Server authorisation token
            AppendSecurityTokenSegment();

            // Append the reportDefinitionDocument
            AppendReportDefinitionSegment(reportDefinitionDocument);
        }

        public XmlDocument PostRequest()
        {
            byte[] bytes = null;
            HttpWebRequest request = WebRequest.Create(m_HttpConnectionString) as HttpWebRequest;
            if (request != null)
            {
                request.Timeout = 3600000;
                request.Method = "POST";
                request.ContentType = "multipart/form-data; boundary=CSMarsXMLHttp_MIME_BOUNDARY";
                if (m_HttpConnectionString.StartsWith("http:"))
                {
                    request.Credentials = CredentialCache.DefaultCredentials;
                }
                else if (m_HttpConnectionString.StartsWith("https:"))
                {  
                  request.ClientCertificates = CertificateMgr.GetPersonalCertificates();
                }
            }
            bytes = System.Text.Encoding.UTF8.GetBytes(HttpXmlRequest.createMimeMessage(this));
            request.ContentLength = bytes.Length;
            using (Stream requestStream = request.GetRequestStream())
            {
                requestStream.Write(bytes, 0, bytes.Length);
            }
            HttpWebResponse serverResponse = request.GetResponse() as HttpWebResponse;
            XmlDocument resultDocument = new XmlDocument();
            XmlTextReader responseReader = new XmlTextReader(serverResponse.GetResponseStream());
            try
            {
                resultDocument.Load(responseReader);
                if (resultDocument.DocumentElement.OuterXml == null || resultDocument.DocumentElement.OuterXml == String.Empty)
                {
                    throw new XmlException("<EmptyDefinition>" + this.IdentifyingName + "</EmptyDefinition>");
                }
                XPathNodeIterator errorNode = resultDocument.CreateNavigator().Select(@"/Replies/*[@status='error']//Error");
                StringBuilder errorMessage = new StringBuilder();
                while (errorNode.MoveNext())
                {
                    errorMessage.Append(errorNode.Current.Value);
                    errorMessage.Append(Environment.NewLine);
                }
                errorNode = resultDocument.CreateNavigator().Select(@"/Reply[@status='error']//Error");
                while (errorNode.MoveNext())
                {
                    errorMessage.Append(errorNode.Current.Value);
                    errorMessage.Append(Environment.NewLine);
                }
                if (errorMessage.Length > 0)
                {
                    throw new ApplicationException(String.Format("The XML returned from MARS contains errors. The text of these errors are: {0}", errorMessage));
                }
            }
            catch (XmlException e)
            {
                resultDocument.LoadXml(e.Message);
            }
            catch (Exception e)
            {
                string message = e.Message;
                throw;
            }
            finally
            {
                //MarsLogManager.AddLog(this, MarsLogManager.MarsRequestType.MarsReportServer, m_UserName);
            }
            return resultDocument;
        }

        private void AppendRoot()
        {
            m_RootElement = this.CreateElement("ROOT");
            this.CreateProcessingInstruction("xml", "version=\"1.0\"");
            this.AppendChild(m_RootElement);
        }

        private void AppendRequestDocumentSegment(XmlDocument requestDocument, bool isSingleRequest)
        {
            XmlElement requestData = this.CreateElement("DATA");
            XmlAttribute requestDataName = this.CreateAttribute("name");
            requestDataName.Value = "request";
            requestData.Attributes.Append(requestDataName);
            m_RootElement.AppendChild(requestData);

            if (requestDocument.SelectSingleNode("/Requests") == null)
            {
                if (isSingleRequest)
                    requestData.InnerXml = requestDocument.DocumentElement.OuterXml;
                else
                    requestData.InnerXml = "<Requests>" + requestDocument.DocumentElement.OuterXml + "</Requests>";
            }
            else
            {
                requestData.InnerXml = requestDocument.DocumentElement.OuterXml;
            }
        }

        private void AppendSecurityTokenSegment()
        {
            XmlElement securityData = this.CreateElement("DATA");
            XmlAttribute securityDataName = this.CreateAttribute("name");
            securityDataName.Value = "authorisation_token";
            securityData.Attributes.Append(securityDataName);
            m_RootElement.AppendChild(securityData);

            XmlNode securityTokenNode = this.ImportNode(Config.SecurityToken(m_UserName, m_GroupName).DocumentElement, true);
            securityData.AppendChild(securityTokenNode);
        }

        private void AppendReportDefinitionSegment(XmlDocument reportDefinitionDocument)
        {
            XmlElement requestData = this.CreateElement("DATA");
            XmlAttribute requestDataName = this.CreateAttribute("name");
            requestDataName.Value = "document";
            requestData.Attributes.Append(requestDataName);
            m_RootElement.AppendChild(requestData);

            XmlNode reportDefinitionDocumentDENode = this.ImportNode(reportDefinitionDocument.DocumentElement, true);
            requestData.AppendChild(reportDefinitionDocumentDENode);
        }

        public static string createMimeMessage(XmlDocument httpRequestDocument)
        {
            StringBuilder mimeMessage = new StringBuilder();

            mimeMessage.Append("MIME-Version: 1.0" + Environment.NewLine);
            //mimeMessage.Append("Content-Type: multipart/form-data; boundary=CSMarsXMLHttp_MIME_BOUNDARY");
            //mimeMessage.Append(newLine);
            //mimeMessage.Append(newLine);

            mimeMessage.Append("This is the preamble area of a multipart message. Mail readers " + Environment.NewLine);
            mimeMessage.Append("that understand multipart format should ignore this preamble." + Environment.NewLine);
            mimeMessage.Append("If you a reading this text, you might want to consider changing" + Environment.NewLine);
            mimeMessage.Append("to a mail reader that understands how to properly display" + Environment.NewLine);
            mimeMessage.Append("multipart messages." + Environment.NewLine);
            mimeMessage.Append(Environment.NewLine);

            foreach (XmlNode data in httpRequestDocument.DocumentElement.ChildNodes)
            {
                mimeMessage.Append("--CSMarsXMLHttp_MIME_BOUNDARY" + Environment.NewLine);
                mimeMessage.Append("Content-Disposition: form-data; name=\"" + data.Attributes["name"].Value + "\"" + Environment.NewLine);
                mimeMessage.Append("Content-Type: text/xml" + Environment.NewLine);
                mimeMessage.Append(Environment.NewLine + data.InnerXml + Environment.NewLine);
            }

            mimeMessage.Append("--CSMarsXMLHttp_MIME_BOUNDARY--" + Environment.NewLine);

            return mimeMessage.ToString();
        }
    }
}


using System;
using System.Collections;
using System.Collections.Generic;

namespace MyWinFormsApp
{
    public class XmlRequestItemCollection : CollectionBase
    {
        public XmlRequestItemCollection() { }

        public XmlRequestItem this[int index]
        {
            get => (List[index] as XmlRequestItem) ?? throw new InvalidCastException($"Item at index {index} is not an XmlRequestItem");
            set => List[index] = value;
        }

        public XmlRequestItem? this[string requestItemName]
        {
            get
            {
                requestItemName = requestItemName.ToLower();
                foreach (XmlRequestItem requestItem in this)
                {
                    if (requestItem.Name.ToLower().Equals(requestItemName))
                        return requestItem;
                }
                return null;
            }
        }

        public int Add(XmlRequestItem requestItem)
        {
            return List.Add(requestItem);
        }

        public void Insert(int index, XmlRequestItem requestItem)
        {
            List.Insert(index, requestItem);
        }

        public void Remove(XmlRequestItem requestItem)
        {
            List.Remove(requestItem);
        }

        public bool Contains(XmlRequestItem requestItem)
        {
            return List.Contains(requestItem);
        }

        public int IndexOf(XmlRequestItem requestItem)
        {
            return List.IndexOf(requestItem);
        }

        public void CopyTo(XmlRequestItem[] array, int index)
        {
            List.CopyTo(array, index);
        }

        public XmlRequestItem[] ToArray()
        {
            XmlRequestItem[] requests = new XmlRequestItem[this.Count];
            this.CopyTo(requests, 0);
            return requests;
        }
    }
}

