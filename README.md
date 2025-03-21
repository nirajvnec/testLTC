async function loadEmailStatusData(): Promise<void> { 
    try {
        const data = await service.getEmailStatusData();
        
        // Ensure the data is valid and replace null/undefined with default values
        setEmailData({
            sent: data.sent ?? 0,
            pending: data.pending ?? 0,
            failed: data.failed ?? 0
        });
    } catch (error) {
        console.error("Failed to load email status data:", error);
    }
}
