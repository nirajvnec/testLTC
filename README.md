const [readOnly, setReadOnly] = useState<boolean>(
        location.state?.readOnly ?? props.readOnly ?? false
    );
