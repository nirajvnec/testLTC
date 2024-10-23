string xmlContent = @"<ROOT>
    <DATA name=""request"">
        <Requests>
            <GetManagedDataItemGroups username=""nkuma152"" type=""Report Definition""/>
        </Requests>
    </DATA>
    <DATA name=""authorisation_token"">
        <MaRSNetSession>
            <LogOnDetails>
                <User name=""nkuma152"">
                    <Groups>
                        <Group name=""MaRS Net Report Control""/>
                    </Groups>
                </User>
            </LogOnDetails>
            <Signature/>
        </MaRSNetSession>
    </DATA>
</ROOT>";
